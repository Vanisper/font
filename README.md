# Font

操作系统字体推荐（编程向）：`Maple Mono` + `JetBrainsMono`

### Maple Mono

https://github.com/subframe7536/maple-font

> 国人出品，集众大家之长，支持中文的等宽（中英等宽比 2:1）。

### JetBrainsMono

https://github.com/JetBrains/JetBrainsMono

> 饱满且圆润，常见的易混淆的字符容易区分（`1li`、`0O`、`;:`）
>
> 支持字符连融

### FiraCode

https://github.com/tonsky/FiraCode

> 中文不支持等宽，更推荐使用 Maple Mono

## Q.A.

### 如何安装

操作系统中安装的字体是 ttf 文件格式的，windows 中可选择中任意多个 ttf，然后右键菜单中会出现 **“安装”** 的字样。

如果是 vscode 或者其他开发编辑器，配置办法详见 [maple-font#usage](https://github.com/subframe7536/maple-font/blob/variable/source/features/README.md#usage) 。

**VSCode settings**：
``` json
{
  // ...其他配置
  "editor.fontFamily": "Maple Mono NF CN, Menlo, Consolas, Maple UI, PingFang, 'Microsoft YaHei', monospace",
  "editor.fontLigatures": true,
  // ...其他配置
}
```

---

值得注意的是 windows 下的终端环境（powershell & cmd）中文乱码问题：

> 
> 这个问题出现的原因其实无关字体。当然字体需要支持中文只是很小的一个原因，但是 windows 默认的终端在中文系统中，设置的字体是宋体，默认就不存在这个问题。
>
> 仍然出现中文乱码问题，可能是因为文本编码是 utf-8 的。
>
> 而 windows 的系统区域（system locale）设置为中文简体的时候，全局默认的字符编码是 GBK 的，需要手动勾选 UTF-8 选项才可以，终端(powershell & cmd)是跟随这个编码的。
> 
> 但是由于历史遗留问题，会导致一些非Unicode中文软件乱码（一般是一些老的软件），详见[此处](https://www.zhihu.com/question/54724102/answer/380875686)，以及下面的讨论。
>

以下是一些解决办法：

1. 对于 powershell，有一些命令是可以通过传入一个 `-Encoding utf8` 的附加参数实现指定输出字符编码

- e.g. `app.log` 是一个含有中文字符的 utf-8 的文本文件
  ```powershell
  cat .\all.log -Encoding utf8
  Get-Content .\all.log -Tail 20 -wait -Encoding utf8
  ```
- [方案来源](https://www.zhihu.com/question/54724102/answer/2325552664)

2. 【临时方案】设置 Active code page - `chcp 65001`

- 在 windows 的终端执行 `chcp`，默认的输出为 **936**，即 GBK；执行 `chcp 65001`，即设置为 **UTF-8**。
  > 这个方案在 cmd 和 powershell 的表现不一致，在 cmd 中表现良好，但比较 dirty，存在作用域污染的问题，在终端中执行 `chcp 65001` 之后，当前 cmd 终端关闭之前的所有中文输出都正常了。
  >
  > 但是在 powershell 下，如果是执行的 bat \ cmd 脚本中存在中文字符乱码，必须得在脚本的开始写入 `chcp 65001` 才可以，并且脚本执行结束之后就不起作用了（但是 CodePage 是真真实实修改了的），这应该是 powershell 在执行脚本时有某种作用域隔离的机制。
  >
  > 所以这个方案的最佳实践应该是在脚本的开始写入 `chcp 65001`，例如：
  > ``` cmd
  > @echo off & chcp 65001
  > echo test chinese character view  测试中文字符显示
  > pause
  > ```
  >
- [来源1](https://www.jianshu.com/p/9f349a9ee77a)
- [来源2](https://blog.csdn.net/runAndRun/article/details/103072938)

3. 针对批处理脚本 —— `.cmd \ .bat`

- 在编写完脚本之后，可以将其用记事本另存为 ANSI 编码，本质上是将文本保存为 GBK 编码。
- powershell 脚本暂未测试

---

以上没有一个完美的方案，但是对于编辑器，以 vscode 为例，可以有比较好的方案。

vscode 配置如下（来源 <https://blog.csdn.net/lzyws739307453/article/details/89823900>）：
``` json
{
  // ...其他配置
  "terminal.integrated.profiles.windows": {
    "Command Prompt": {
        "path": "C:\\Windows\\System32\\cmd.exe",
        "args": ["-NoExit", "/K", "chcp 65001"]
    },
    "PowerShell": {
        "source": "PowerShell",
        "args": ["-NoExit", "/C", "chcp 65001"]
    }
  },
  // 按喜好配置首选终端
  // "terminal.integrated.defaultProfile.windows": "Command Prompt",
  // ...其他配置
}
```

可以看到，此方案还是通过设置 CodePage，比较疑惑的是，PowerShell 如此设置也能生效。

这是因为上面的写法相当于是在启动终端作用域之前就设置好了 CodePage，而不是在作用域之内设置的，具有极高的优先级。
>
> 上面的配置分别相当于在终端中执行以下命令（无所谓 powershell 还是 cmd）:
> ```powershell
> powershell -NoExit /C "chcp 65001" # 引号可以不加，下同
> ```
> ```cmd
> cmd -NoExit /K "chcp 65001"
> ```
>

但是 powershell 的一些指令还是可能会乱码，如：
``` powershell
cat .\all.log # 如果含有中文，此时可能还会有乱码
```
此时需要附加 `-Encoding utf8` 参数：
``` powershell
cat .\all.log -Encoding utf8 # 解决乱码问题
```

> 该方案下的类似方案，解决一些编程语言开发时的编辑器终端乱码：https://blog.csdn.net/qq_43700348/article/details/130269369

---

### ttf 和 woff 的区别

> OTF TTF .otf .ttf 之间的区别，见：https://zhuanlan.zhihu.com/p/386035885
>
> 以下来源于 deepseek

#### **TTF vs WOFF/WOFF2 对比总结**

| 特性         | TTF                 | WOFF                 | WOFF2                 |
|--------------|---------------------|----------------------|-----------------------|
| **压缩**     | 无或轻度压缩        | ZIP压缩（减30%-40%） | Brotli压缩（再减30%） |
| **文件体积** | 较大                | 中等                 | 最小                  |
| **兼容性**   | 所有现代系统/浏览器 | IE9+，主流浏览器     | 新浏览器（无IE）      |
| **用途**     | 桌面软件、打印设计  | 网页标准格式         | 高性能网页优化        |
| **元数据**   | 不支持              | 支持（授权信息等）   | 支持                  |

#### **使用建议**
- **网页开发**：优先加载 `WOFF2` → `WOFF` → `TTF`（兜底）。
- **本地设计**：直接使用 `TTF/OTF`。
- **老旧系统**：需保留 `TTF` 兼容。

---

#### **详细对比说明**

##### **设计目的**

- **TTF**
  - **传统字体格式**：最初由Apple和Microsoft开发，用于操作系统和桌面应用程序（如Word、Photoshop）。
  - **功能**：支持基本的字体渲染，包含轮廓数据（TrueType曲线）、字符映射表等。

- **WOFF/WOFF2**
    - **专为网页优化**：由W3C标准化的网络字体格式，旨在解决TTF/OTF在网页加载时的效率问题。
    - **压缩与元数据**：本质上是TTF/OTF的压缩封装格式，添加了网页所需的元数据（如授权信息）。

##### **压缩与文件大小**

- **TTF**
  - **无压缩或轻度压缩**：文件较大，可能影响网页加载速度。

- **WOFF**
    - **压缩率较高**：使用ZIP压缩，比TTF体积减小约30%-40%。

- **WOFF2**
    - **更高效的压缩**：采用Brotli算法，比WOFF再减小30%左右，适合带宽敏感的场景。

##### **兼容性**

- **TTF**
  - **广泛支持**：所有现代浏览器和操作系统均兼容，但老旧浏览器（如IE8）需要特定配置。

- **WOFF**
    - **主流浏览器支持**：IE9+、Chrome、Firefox、Safari等均支持。

- **WOFF2**
    - **较新浏览器**：Chrome 36+、Firefox 39+、Edge 14+，**不支持IE**。

##### **功能支持**

- **TTF**
  - 支持基本字体特性（如抗锯齿、Hinting），但缺乏针对网页的优化。

- **WOFF/WOFF2**
  - 保留TTF/OTF的所有功能，同时支持：
    - **子集化**（仅包含需要的字符，进一步减小体积）。
    - **元数据嵌入**（如字体作者、许可证信息）。

##### **使用场景**

- **TTF**
  - 适合桌面软件、打印设计或需要向后兼容的环境。
  - 本地安装字体（如设计师使用的字体库）。

- **WOFF/WOFF2**
  - **网页首选**：性能优化，减少HTTP请求时间。
  - 响应式设计、移动端网页（WOFF2尤其适合）。

---

##### **如何选择？**

1. **网页开发**：优先使用WOFF2（兼容性允许时），回退到WOFF，最后TTF。

```css
@font-face {
  font-family: 'MyFont';
  src: url('font.woff2') format('woff2'),
       url('font.woff') format('woff'),
       url('font.ttf') format('truetype');
}
```

2. **设计/打印**：直接使用TTF或OTF（保留完整功能）。

3. **老旧系统**：TTF作为兜底方案。

---

### variable_ttf 和 ttf 有什么区别

> 

详见：https://github.com/tonsky/FiraCode/discussions/1275

---

