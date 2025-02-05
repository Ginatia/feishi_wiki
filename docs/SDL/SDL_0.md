# SDL2 Note

## SDL2 main macro

注意到 SDL2 库有一个名为 main的宏

其在 SDL_main.h 里

而我们的入口函数也为main

这个宏会将main 替换为 SDL_main

导致找不到入口函数main

查看 SDL_main.h 可知, 

#define SDL_MAIN_HANDLED 

可避免定义此main宏

```cpp
#define SDL_MAIN_HANDLED
#include <SDL2/SDL.h>
```

## Init SDL

```cpp
SDL_Init( SDL_INIT_EVERYTHING );
```

## 创建窗口

参数列表依次为:

窗口名称(const char*),位置(int x,int y),大小(int w,int h),选项的参数(Uint32 flags)

```cpp
SDL_Window* win = SDL_CreateWindow( "my window", 100, 100, 640, 480, SDL_WINDOW_SHOWN );
```


## 获取SDL2 错误

```cpp
std::cout<<SDL_GetError();
```

## Surfaces 表面

在 SDL 中创建窗口后,使用 SDL_Surface 管理窗口的绘制

```cpp
SDL_Surface* winSurface = SDL_GetWindowSurface( win );

// 对 winSurface 进行绘制之后,
// 使用 SDL_UpdateWindowSurface 在窗口中查看结果

SDL_UpdateWindowSurface( win );

```

## 绘制矩形

参数列表为:

(SDL_Surface * dst, const SDL_Rect * rect, Uint32 color)

若需填充整个窗口, 参数 SDL_Rect* 填入 NULL 即可

```cpp
SDL_FillRect( winSurface, NULL, SDL_MapRGB( winSurface->format, 255, 90, 120 ));
```

## 释放资源

注意到在 SDL 中使用指针变量管理资源

有开就有关

SDL 提供了对应的函数以释放资源

```cpp
SDL_DestroyWindow( win );
win = NULL;
winSurface = NULL;

// 最后关闭所有SDL资源
SDL_Quit();
```

## 结合智能指针

由于 SDL 库存在很多手动调用释放资源的函数

不妨结合使用智能指针来管理资源

```cpp
template<typename T,void(*Deleter)(T*)>
struct SDL_Deleter {
	void operator()(T* ptr)const {
		if (ptr) {
			Deleter(ptr);
		}
	}
};

namespace SDL {
	using WindowPtr = std::unique_ptr<SDL_Window, SDL_Deleter<SDL_Window, SDL_DestroyWindow>>;
	using SurfacePtr = std::unique_ptr<SDL_Surface, SDL_Deleter<SDL_Surface,SDL_FreeSurface>>;
}
```