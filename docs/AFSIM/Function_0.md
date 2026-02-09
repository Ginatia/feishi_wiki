# 插件执行afsim脚本函数

## 封装获取afsim脚本函数方法

```cpp

class Foo : public WsfSimulationExtension{
public:
	Foo();
	Foo(const Foo& aSrc);
	Foo& operator=(const Foo& aRhs);
	~Foo()noexcept override;
	
	bool Initialize()override;
	
	const std::string execute_method(const std::string& aMethodName,const UtScriptDataList& aArgs)const;
	void PrintName(double aSimTime);
	
private:
	double runTimeLeft;
	UtCallbackHolder mCallbacks;
};

```

此类为插件的场景扩展，通过回调函数在仿真中执行 PrintName 方法。

其中 execute_method 方法获取脚本中的 get_name 函数。

```cpp

void Foo::PrintName(double aSimTime){
	if(aSimTime < runTimeLeft + 2.0){
		return;
	}
	runTimeLeft = aSimTime;
	
	auto eventPtr = ut::make_unique<WsfOneShotEvent>(aSimTime,[&](){
		UtScriptDataList args;	
		std::string data = execute_method("get_name",args);
		std::cout << data << "\n";
	});
	GetSimulation().AddEvent(std::move(eventPtr));
}

```


将函数包装为 lambda 表达式，以接受 WsfOneShotEvent 类的构造函数定义。

WsfOneShotEvent 类的构造函数第一个参数为执行函数时间，第二个为执行函数对象。

第二个参数接受类型为： `std::function<RT(Args...)>` 最方便的是使用 lambda 表达式进行包装。

WsfOneShotEvent 继承自 WsfEvent,作用为执行一次即删除。

PrintName 函数根据其函数签名可以与对应的 Observer 绑定。

如 WsfObserver::AdvanceTime 。

若需要更多的执行功能，通过继承 WsfEvent,自定义即可。

## WsfEvent

通过继承 WsfEvent, 自定义事件执行。

注册一个事件，执行输出 "Foo Event\n" 字符串。

每 5 s 执行一次，直到时间 >= 100 s

```

class Foo : public WsfSimulationExtension{
public:
	bool Initialize()override;
	
	class FooEvent : public WsfEvent{
	public:
		~FooEvent()override = default;
		FooEvent(double aSimTime,Foo* aFoo);
		EventDisposition Execute()override;
		
		Foo* mFoo;
	};
	
	WsfScriptContext* mContextPtr{ nullptr };
};

```

在 Foo::Initialize 函数中获取 WsfScriptContext* 指针。

因为引擎调用此函数时，访问的 WsfSimulation* 指针不为空。

```cpp

bool Foo::Initialize(){
	if(&GetSimulation() == nullptr){
		std::cout << "Null Ptr\n";
		return false;
	}
	
	mContextPtr = &(GetSimulation().GetScriptContext());
	
	double simTime = GetSimulation().GetSimTime();
	double nextTime = simTime + 5.0;
	auto eventPtr = ut::make_unique<FooEvent>(nextTime,this);
	GetSimualtion().AddEvent(std::move(eventPtr));
	
	return true;
}

```

EventDisposition 是 WsfEvent 类的枚举。

若返回 WsfEvent::cDELETE 则说明此函数结束执行。

若返回 WsfEvent::cRESCHEDULE 则说明此函数继续加入事件队列，再次执行。且需要设置再次执行时间。

```cpp

WsfEvent::EventDisposition Foo::FooEvent::Execute(){
	std::cout << "Foo Event\n";
	
	double curTime = GetTime();
	if(curTime >= 100.0){
		return WsfEvent::cDELETE;
	}
	double nextTime = curTime + 5.0;
	SetTime(nextTime);
	return WsfEvent::cRESCHEDULE;
}

```