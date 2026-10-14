# STM32 数据类型转换工具 (STM32 Data Type Converter)

简体中文 | [English](./README.en.md)

基于 Claude Code 浅色极简直角风格构建的 STM32 嵌入式数据类型在线转换工具。
在线访问：<https://lilixxs.github.io/dataConvlilixxs/>

## 核心功能

1. **10 种标准数据类型全量同步**：
   - 有符号整型：`int8`、`int16`、`int32`、`int64`
   - 无符号整型：`uint8`、`uint16`、`uint32`、`uint64`
   - IEEE 754 浮点型：`float`（32 位单精度）、`double`（64 位双精度）
2. **多字节序（Endianness）独立重排**：
   - 16 位支持 `AB`（大端）、`BA`（小端）
   - 32 位支持 `ABCD`（大端）、`DCBA`（小端）、`BADC`（字节交换）、`CDAB`（字交换）
   - 64 位支持 `ABCDEFGH`（大端）、`HGFEDCBA`（小端）、`BADCFEHG`（字节交换）、`GHEFCDAB`（字交换）、`EFGHABCD`（双字交换）
3. **多进制输入与切换**：
   - 支持直接输入十进制、科学计数法、`0x` 十六进制、`0b` 二进制。
   - 支持一键切换全局输出为 10 进制、16 进制或 2 进制。
4. **按字节视觉留白与无空格复制**：
   - 16 进制与 2 进制模式下，界面通过 CSS 外边距按字节分块显示。
   - 点击右侧复制按钮或划选复制时，剪贴板输出无空格连续字符串（如 `0x12345678`），可直接粘贴至 C 代码。
5. **中英文多语言切换**：
   - 界面文案存储于 `i18n/zh-CN.json` 与 `i18n/en-US.json`。
   - 使用帮助文档存储于 `i18n/help.zh-CN.md` 与 `i18n/help.en-US.md`。

## 目录结构

- `lilixxsDataConv.html`：应用主页面（包含 UI 布局、转换逻辑与多语言加载器）。页面通过 HTTP `fetch` 从 `i18n/` 目录读取外部语言包与帮助文档。
- `index.html`：GitHub Pages 根路径重定向入口。
- `i18n/zh-CN.json`：简体中文界面语言包。
- `i18n/en-US.json`：英文界面语言包。
- `i18n/help.zh-CN.md`：简体中文使用帮助（Markdown 格式）。
- `i18n/help.en-US.md`：英文使用帮助（Markdown 格式）。
- `claude_code_light_ui_style_guide.md`：UI 视觉规范参考文档。
- `README.en.md`：英文项目说明文档。
- `CONTRIBUTING.md`：贡献与多语言翻译维护指南。
- `LICENSE`：MIT 开源协议文本。

## 本地运行

页面通过 HTTP `fetch` 直接读取 `i18n/` 目录下的外部语言包与 Markdown 帮助文档。
本地预览需先启动 HTTP 静态服务器：

```bash
python -m http.server 8000
```

在浏览器访问 `http://localhost:8000/lilixxsDataConv.html`。

## 贡献与多语言维护

新增功能或新增语种时，请同步更新 `i18n/*.json` 与 `i18n/help.*.md`。详细字段规范请参阅 [CONTRIBUTING.md](./CONTRIBUTING.md)。

## 版权与开源协议

作者：[lilixxs](https://github.com/lilixxs/dataConvlilixxs)
本项目基于 [MIT License](./LICENSE) 开源。
