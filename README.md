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

