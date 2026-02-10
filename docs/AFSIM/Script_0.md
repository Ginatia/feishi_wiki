# afsim 脚本关键词

## static

static 关键词用于声明变量，此变量只赋值一次。

如下代码片段中，time_1 变量每2s打印一次，但值都为第一次赋值的值。

```
execute at_interval_of 2 s
	static double time_1 = TIME_NOW;
	writeln(write_str("time_1: ",time_1));
end_execute

```

## 调用对象上的用户自定义脚本

“->”作符用于调用对象上的用户自定义脚本，以及获取/设置对象上的用户自定义脚本变量。

```
platform test WSF_PLATFORM
	script void print_name()
		writeln("test");
	end_script
end_platform

execute at_interval_of 2 s
	WsfPlatform p = WsfSimulation.FindPlatform("test");
	p->print_name();
end_execute

```

