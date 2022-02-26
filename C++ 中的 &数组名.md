<!--
 * @Author: JinBridge
 * @Date: 2022-02-18 20:13:14
 * @LastEditors: JinBridge
 * @LastEditTime: 2022-02-26 16:21:59
 * Copyright (c) 2022 by JinBridge, All Rights Reserved. 
-->
# C++ 中的 &数组名

## 先看一个二维数组
不妨先看一段代码。

```cpp
int arr[10][10];
printf("arr      = %p\n", arr);
printf("arr[0]   = %p\n", arr[0]);
printf("arr+1    = %p\n", arr+1);
printf("arr[0]+1 = %p\n", arr[0]+1);
```

输出为

```shell
arr      = 0x0000000061fc90
arr[0]   = 0x0000000061fc90
arr+1    = 0x0000000061fcb8
arr[0]+1 = 0x0000000061fc94
```
可以看出来，`arr[0]` 与 `arr` 虽然值相同，但是它们移动的单位不同，严格意义上它们的类型也不同。

`arr[0]` 的类型为 `int*`
`arr`    的类型为 `int(*)[10]`

## 如果 `arr` 是个一维数组呢？

仍然是一段代码。

```cpp
int arr[10];
printf("arr    = %p\n", arr);
printf("&arr   = %p\n", &arr);
printf("arr+1  = %p\n", arr+1);
printf("&arr+1 = %p\n", &arr+1);
```

输出为

```shell
arr    = 0x0000000061fdf0
&arr   = 0x0000000061fdf0
arr+1  = 0x0000000061fdf4        
&arr+1 = 0x0000000061fe18
```

对比之前看的，不难理解。

`arr` 的类型为 `int*`
`&arr`    的类型为 `int(*)[10]`

所谓的 `&arr` 实际上就是指向 `arr` 的指针，与指向 `int` 的 `arr` 不同之处就是类型不同，一个是 `int(*)[10]` 而另一个是 `int*` ，因此它们移动的单位也不同，前一个 `+1` 移动 40 字节而后一个移动 4 字节。