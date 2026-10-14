# AGENTS.md

## 项目架构

本仓库托管部署于 GitHub Pages 的纯静态单页应用（STM32 数据类型转换器）。
本仓库不使用包管理器、构建工具或外部运行依赖。

- `lilixxsDataConv.html`：应用主文件，包含 HTML 结构、CSS 样式、数值转换逻辑与多语言加载器。页面通过 HTTP `fetch` 直接读取外部 `i18n/*.json` 与 `i18n/help.*.md` 文件。
- `index.html`：GitHub Pages 根路径入口文件，自动重定向至 `./lilixxsDataConv.html`。
- `i18n/zh-CN.json`、`i18n/en-US.json`：中英文界面翻译文本（外部 JSON 文件）。
- `i18n/help.zh-CN.md`、`i18n/help.en-US.md`：中英文使用帮助文档（外部 Markdown 文件）。
- `claude_code_light_ui_style_guide.md`：UI 视觉规范参考文档。
- `README.md`、`CONTRIBUTING.md`、`LICENSE`：项目说明、翻译贡献指南与 MIT 协议文件。

## 真实数据源优先级

当 `claude_code_light_ui_style_guide.md` 与 `lilixxsDataConv.html` 冲突时，以 `lilixxsDataConv.html` 为准。

- **配色、字体与直角规范**：遵循 `claude_code_light_ui_style_guide.md`。所有组件保持 `border-radius: 0`，画布背景使用 `#FAF9F5`，主强调色使用 `#D97757`。
- **栅格、尺寸与组件结构**：遵循 `lilixxsDataConv.html`。容器最大宽度为 `1080px`，移动端断点为 `920px`。主面板包含「字节序」列、复制按钮与右侧帮助抽屉，采用五列栅格（`180px 185px 155px minmax(250px, 1fr) 205px`）。禁止按样式指南回退为 `980px` 四列布局。

## 核心代码不变量

修改代码或新增功能时，必须遵守以下规则：

1. **同步更新外部多语言与帮助文件**：
   - 所有界面文案存储在 `i18n/zh-CN.json` 与 `i18n/en-US.json`。
   - 所有使用帮助内容存储在 `i18n/help.zh-CN.md` 与 `i18n/help.en-US.md`。
   - 今后添加新功能或修改文案时，只需同步更新上述外部 JSON 与 Markdown 文件。页面通过 HTTP `fetch` 直接读取这些外部文件。
2. **保持零外部依赖**：禁止引入外部 CSS、JS 框架或字体 CDN。
3. **保持 `contenteditable` 输入契约**：所有 `.type-input` 元素使用 `<div contenteditable="true">`。初始化脚本通过 `Object.defineProperty(inputEl, "value", ...)` 挂载 `.value` 属性，读取 `.value` 时自动剔除全部空白字符。
4. **保持按字节视觉留白与无空格复制**：
   - 16 进制与 2 进制渲染将每个字节包裹在 `<span class="byte-chunk">` 内，仅通过 CSS 规则 `.byte-chunk + .byte-chunk { margin-left: 0.55em }` 产生视觉间距。
   - 禁止在 DOM 文本节点中插入空格字符。划选复制事件（`copy`）与复制按钮（`.copy-btn`）必须输出无空格连续字符串（如 `0x12345678`）。
5. **保持数值转换与字节序算法**：
   - `int64` 与 `uint64` 使用 `BigInt` 运算，防止超出 IEEE 754 双精度安全整数范围时丢失精度。
   - `float` 与 `double` 通过共享的 8 字节 `ArrayBuffer`（`buf8`）与 `DataView`（`view8`）读写 IEEE 754 字节。
   - 标准大端序（高字节在前）依次记为 `A` 至 `H`。`reorderBytes` 与 `unreorderBytes` 均以大端字节数组为基准重排字节。
   - 浮点数转整型使用 `Math.trunc` 向零截断。
   - 当目标类型溢出时，目标框显示当前语言的 `status.outOfRange` 文本（中文 `"超范围"` / 英文 `"Out of range"`）并添加 `.out-of-range-val` 类，同时保持 `lastConversionValid` 为 `true`。当源输入越界或格式非法时，将 `lastConversionValid` 设为 `false`，并在当前行显示 `.status-err`。

## 本地验证与部署

1. **启动本地预览**：执行 `python -m http.server 8000` 后访问 `http://localhost:8000/lilixxsDataConv.html`，验证外部 `i18n/*.json` 与 `i18n/help.*.md` 通过 HTTP 正常加载。
2. **执行回归检查**：
   - 页面加载时默认计算 `uint8 = 200`，`int8` 显示超范围状态，其余 8 种类型显示 `200`。
   - 切换中文（`zh-CN`）与英文（`en-US`）时，顶栏、表头、类型说明、字节序选项、超范围文本、状态提示、帮助抽屉 Markdown 渲染及页脚版权信息全部同步切换。
   - 切换 10/16/2 进制或切换单行字节序时，仅重排有效单元格，不覆盖超范围状态。
   - 在 16 进制或 2 进制模式下点击复制按钮，剪贴板内容不含空格。
   - 页脚作者信息链接指向 `https://github.com/lilixxs/dataConvlilixxs`。
3. **保持 GitHub Pages 入口**：`index.html` 负责将根路径请求重定向至 `./lilixxsDataConv.html`。不要删除 `index.html`。
