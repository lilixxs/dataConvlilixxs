# AGENTS.md

## 项目架构

本仓库托管部署于 GitHub Pages 的纯静态单页应用（STM32 数据类型转换器）。
本仓库不使用包管理器、构建工具或外部运行依赖。

- `lilixxsDataConv.html`：应用主文件，内联全部 HTML 结构、CSS 样式与原生 JavaScript 代码。
- `index.html`：GitHub Pages 根路径入口文件，自动重定向至 `./lilixxsDataConv.html`。
- `claude_code_light_ui_style_guide.md`：UI 视觉规范参考文档。

## 真实数据源优先级

当 `claude_code_light_ui_style_guide.md` 与 `lilixxsDataConv.html` 冲突时，以 `lilixxsDataConv.html` 为准。

- **配色、字体与直角规范**：遵循 `claude_code_light_ui_style_guide.md`。所有组件保持 `border-radius: 0`，画布背景使用 `#FAF9F5`，主强调色使用 `#D97757`。
- **栅格、尺寸与组件结构**：遵循 `lilixxsDataConv.html`。容器最大宽度为 `1080px`，移动端断点为 `920px`。主面板包含「字节序」列、复制按钮与右侧帮助抽屉，采用五列栅格（`180px 185px 155px minmax(250px, 1fr) 205px`）。禁止按样式指南回退为 `980px` 四列布局。

## 核心代码不变量

修改 `lilixxsDataConv.html` 时，必须遵守以下规则：

1. **保持单文件零依赖**：将所有 CSS 写入 `<style>` 标签，将所有 JavaScript 写入 `<script>` 标签。禁止引入外部样式表、脚本文件或字体 CDN。
2. **保持 `contenteditable` 输入契约**：所有 `.type-input` 元素使用 `<div contenteditable="true">`。初始化脚本通过 `Object.defineProperty(inputEl, "value", ...)` 挂载 `.value` 属性，读取 `.value` 时自动剔除全部空白字符。
3. **保持按字节视觉留白与无空格复制**：
   - 16 进制与 2 进制渲染将每个字节包裹在 `<span class="byte-chunk">` 内，仅通过 CSS 规则 `.byte-chunk + .byte-chunk { margin-left: 0.55em; }` 产生视觉间距。
   - 禁止在 DOM 文本节点中插入空格字符。划选复制事件（`copy`）与复制按钮（`.copy-btn`）必须输出无空格连续字符串（如 `0x12345678`）。
4. **保持数值转换与字节序算法**：
   - `int64` 与 `uint64` 使用 `BigInt` 运算，防止超出 IEEE 754 双精度安全整数范围时丢失精度。
   - `float` 与 `double` 通过共享的 8 字节 `ArrayBuffer`（`buf8`）与 `DataView`（`view8`）读写 IEEE 754 字节。
   - 标准大端序（高字节在前）依次记为 `A` 至 `H`。`reorderBytes` 与 `unreorderBytes` 均以大端字节数组为基准重排字节。
   - 浮点数转整型使用 `Math.trunc` 向零截断。
   - 当目标类型溢出时，目标框显示 `"超范围"` 并添加 `.out-of-range-val` 类，同时保持 `lastConversionValid` 为 `true`。当源输入越界或格式非法时，将 `lastConversionValid` 设为 `false`，并在当前行显示 `.status-err`。

## 本地验证与部署

1. **启动本地预览**：直接在浏览器中打开 `lilixxsDataConv.html`，或执行 `python -m http.server 8000` 后访问 `http://localhost:8000/lilixxsDataConv.html`。
2. **执行回归检查**：
   - 页面加载时默认计算 `uint8 = 200`，`int8` 显示 `超范围`，其余 8 种类型显示 `200`。
   - 切换 10/16/2 进制或切换单行字节序时，仅重排有效单元格，不覆盖 `超范围` 状态。
   - 在 16 进制或 2 进制模式下点击复制按钮，剪贴板内容不含空格。
3. **保持 GitHub Pages 入口**：`index.html` 负责将根路径请求重定向至 `./lilixxsDataConv.html`。不要删除 `index.html`。
