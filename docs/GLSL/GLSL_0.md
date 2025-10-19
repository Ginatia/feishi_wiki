
# Hello World

```glsl
void main(){
	gl_FragColor = vec4(1.0, 0.0, 1.0, 1.0);
}
```

最终的像素颜色取决于glslViewer预设的全局变量 `gl_FragColor`

此处的 vec4 描述 rgba，范围为 $[0.0,1.0]$
与 $[0,255]$ 范围的颜色分量不一样，若转换则除以 255。

```glsl
vec4 red(){
	return vec4(1.0, 0.0, 0.0, 1.0);
}

void main(){
	gl_FragColor = red();
}
```

语法类似 C语言，可设置函数简化主流程。

```glsl
uniform float u_time;

void main(){
	gl_FragColor = vec4(abs(sin(u_time)), 0.0, 0.0, 1.0);
}
```

u_time 是 glslViewer 程序定义并传递的，并非glsl内置变量。
在glsl中声明，并在正文使用。

## 输入

```glsl
uniform vec2 u_resolution; // 画布尺寸（宽，高）
uniform vec2 u_mouse; // 鼠标位置（在屏幕上哪个像素）
uniform float u_time; // 时间（加载后的秒数）

void main() {
	vec2 st = gl_FragCoord.xy / u_resolution;
	gl_FragColor = vec4(st.x,st.y,0.0,1.0);
}
```

`gl_FragCoord` 存储了活动线程正在处理的**像素**或**屏幕碎片**的坐标。
上述代码中用 `gl_FragCoord.xy` 除以 `u_resolution`，对坐标进行了**规范化**。