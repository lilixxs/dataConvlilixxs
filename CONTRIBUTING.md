# 贡献与多语言翻译指南 (Contributing & Translation Guide)

本文档说明如何为本项目贡献代码、新增功能以及维护多语言文件。

## 1. 多语言文件结构

项目采用外部 JSON 语言包与外部 Markdown 帮助文档分离的设计：

- `i18n/zh-CN.json`：简体中文 UI 界面键值表。
- `i18n/en-US.json`：英文 UI 界面键值表。
- `i18n/help.zh-CN.md`：简体中文使用帮助文档。
- `i18n/help.en-US.md`：英文使用帮助文档。

## 2. 新增功能时的同步规则

向 `lilixxsDataConv.html` 添加新功能、新控件、新状态提示或新转换规则时，必须同步执行以下步骤：

1. 在 `i18n/zh-CN.json` 与 `i18n/en-US.json` 中添加对应的翻译键，保持两个 JSON 文件的键结构完全一致。
2. 在 `i18n/help.zh-CN.md` 与 `i18n/help.en-US.md` 中补充新功能的操作说明或转换规则。

## 3. 新增语种流程

若需添加新语言（例如 `ja-JP`）：

1. 复制 `i18n/en-US.json` 为 `i18n/ja-JP.json`，翻译所有字段值，并将 `"lang"` 字段设为 `"ja-JP"`。
2. 复制 `i18n/help.en-US.md` 为 `i18n/help.ja-JP.md`，翻译章节标题、列表项与图解说明。
3. 在 `lilixxsDataConv.html` 的 `#langGroup` 工具栏中添加语言切换按钮：
   ```html
   <button type="button" class="lang-btn" data-lang="ja-JP">日本語</button>
   ```

## 4. 帮助文档 Markdown 语法规范

`i18n/help.<lang>.md` 使用受控子集语法，由页面内置解析器渲染为 Claude Code 风格组件：

- **章节标题**：使用 `## <两位编号> <标题>`（如 `## 01 快速输入与触发转换`）。
- **说明列表**：使用 `- **小标题：**正文内容`。若需添加次行灰色注释，在下一行缩进编写 `SUB: 注释内容`。
- **内联标记**：
  - `` `Enter` `` 或 `` `↵` ``：渲染为按键标签 `.kbd-tag`。
  - `[COPY_ICON]`：渲染为内联复制图标。
  - `[OK_MARK]`：渲染为绿色成功标记 `√`。
  - `[WARN:文本]`：渲染为橙色警告标签。
- **图解表格**：以 `> // 图解标题` 开头，紧接两列 Markdown 表格。若第二列全部为 `` `A:12` `` 格式的字节块，解析器自动渲染为带字节编号的 `.byte-box` 矩阵。
- **复制演示框**：使用 `> COPY_DEMO: 左侧显示文本 | 箭头文本 | 右侧复制结果`。

## 5. 提交前验证清单

1. 启动本地 HTTP 服务：`python -m http.server 8000`。
2. 访问 `http://localhost:8000/lilixxsDataConv.html`，切换中文与英文，确认所有表头、量程提示、字节序下拉框、超范围提示、状态提示和底部版权信息完整切换。
3. 打开「使用帮助 / Manual」抽屉，确认中英文 Markdown 文档正常渲染且无未解析标记。
4. 验证 16 进制与 2 进制复制结果不含空格。
