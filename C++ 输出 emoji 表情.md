<!--
 * @Author: JinBridge
 * @Date: 2022-02-26 16:07:28
 * @LastEditors: JinBridge
 * @LastEditTime: 2022-02-26 16:21:03
 * Copyright (c) 2022 by JinBridge, All Rights Reserved. 
-->
# C++ 输出 Emoji 表情

## 什么是 Emoji 表情

```
Emoji（日语：絵文字／えもじ emoji），是使用在网页和聊天中的形意符号，最初是日本在无线通信中所使用的视觉情感符号（图画文字）。表情意指面部表情，图标则是图形标志的意思，可用来代表多种表情，如笑脸表示笑、蛋糕表示食物等。在香港除“表情图标”外，也有称作“绘文字”或“emoji”。在台湾LINE软件中，emoji又被叫做“表情贴”。在中国大陆，emoji通常直称“emoji”或称“表情符号”。新马即“绘文字”或直接称“emoji”. —— Wikipedia
```

实际上使用 C++ 输出 Emoji 表情与其他字符串在本质上并没有什么不同，只不过 Emoji 是基于 Unicode 编码。因此我们需要使用支持 Unicode 的终端以及对应的编译器。

## Code

```cpp
std::cout << "😊";
```

上述代码可以在终端中输出一个 Emoji 笑脸表情。

## 为什么输出不了

### Case 0: 编译失败，编译器不支持 Emoji

某些编译器不支持 Emoji / Unicode, 因此在编译时会报错。例如 G++ 等。

解决方案也很简单：使用 Clang++ 等支持 Unicode 的编译器代替即可。

### Case 1: 编译成功，但输出乱码

首先要注意终端是否支持 Emoji. Windows 自带的 Cmd 与 PowerShell 都是 __不支持__ Emoji 的。因此，如果要输出 Emoji 就应当使用 Windows Terminal 等支持 Emoji 的终端运行。

但即使支持 Emoji 也有可能会出现中文乱码，这是因为终端当前使用的编码并不是 Unicode 编码。在 Windows 下使用如下命令将终端编码类型切换为 Unicode UTF-8:

```
chcp 65001
```

再次尝试输出即可。