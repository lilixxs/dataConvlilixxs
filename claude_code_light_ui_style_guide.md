# Claude Code 浅色极简直角 UI 风格指南 (Style Guide)

本指南总结自「STM32 数据类型转换工具」页面的视觉与交互规范，适用于在其他对话中快速复刻**浅色、直角边框、适当留白、精简注释、Claude Code 配色**的Web工具或单页应用。

## 1. 核心设计原则

- **全局直角几何（Zero Border-Radius）**：所有容器、输入框、按钮、标签、分段控制器均采用严格直角（`border-radius: 0`），通过清晰的 `1px solid` 线条构建具有工程师工具感的秩序与利落边界。
- **温润浅色画布（Claude Code Light Palette）**：摒弃刺眼的纯白背景与高饱和度蓝色，采用暖调羊皮纸/米白画布（`#FAF9F5`）搭配纯白内容卡片（`#FFFFFF`）与陶土橙强调色（`#D97757`），兼具代码终端的专业感与长时间阅读的舒适度。
- **充裕留白与呼吸感（Generous Whitespace）**：增大页面四周留白（桌面端 `48px 32px 64px`）与表格行内间距（`13px 24px`），避免元素拥挤堆砌。
- **极简信息层级（Minimalist Annotations）**：去除冗余的说明段落、次要副标题与装饰性徽章，仅保留核心功能名称、标准量程、输入控件与即时状态反馈。

## 2. 配色方案与 CSS 变量

| 语义分类 | 变量名称 | 色值 (HEX / RGBA) | 用途说明 |
| :--- | :--- | :--- | :--- |
| **背景层** | `--bg-canvas` | `#FAF9F5` | 页面全局暖米白底色 |
| **背景层** | `--bg-surface` | `#FFFFFF` | 主卡片、激活态输入框背景 |
| **背景层** | `--bg-subtle` | `#F3F1EA` | 表头、代码标签、次级块背景 |
| **背景层** | `--bg-input` | `#FCFBF9` | 默认输入框微暖底色 |
| **背景层** | `--bg-hover` | `#F7F5EE` | 数据行悬停背景（激活行为 `#FCF7F4`） |
| **边框层** | `--border-soft` | `#E5E2D8` | 行分割线、内部弱边框 |
| **边框层** | `--border-strong` | `#CFCAC0` | 主卡片外框、输入框与按钮边框 |
| **品牌强调色** | `--accent-clay` | `#D97757` | Claude 标志性陶土橙（主选中态、焦点边框） |
| **品牌强调色** | `--accent-clay-dark` | `#BE5B3B` | 深陶土色（终端前缀标识、高亮文字） |
| **品牌强调色** | `--accent-clay-tint` | `#FDF3EF` | 浅陶土底色（当前激活行类型标签背景） |
| **品牌强调色** | `--accent-focus-ring` | `rgba(217, 119, 87, 0.18)` | 输入框聚焦外发光（`2px` 宽度） |
| **文字层级** | `--text-primary` | `#141413` | 标题、输入框数值、核心标签文字 |
| **文字层级** | `--text-secondary` | `#5C5A54` | 副标题、类型中文说明、量程文本 |
| **文字层级** | `--text-muted` | `#8C887E` | 分组小标题、内嵌回车符号、占位符（`#B8B4A9`） |
| **状态语义色** | `--success-text` / `bg` / `border` | `#28663B` / `#EBF5EE` / `#C0E0C8` | 转换完成 / 校验通过状态提示 |
| **状态语义色** | `--error-text` / `bg` / `border` | `#B83525` / `#FDECE9` / `#F3C2BA` | 输入超量程 / 格式错误提示 |
| **状态语义色** | `--warn-text` / `bg` / `border` | `#9E4B24` / `#FDF2EB` / `#F2CFC0` | 目标框“超范围”或警告状态 |

## 3. 字体排印规范

- **UI 无衬线字体栈（`--font-sans`）**：
  `-apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "Noto Sans SC", sans-serif`
  用于页面主标题、副标题、表头、中文类型说明、按钮与状态提示。
- **代码与数值等宽字体栈（`--font-mono`）**：
  `"JetBrains Mono", "SF Mono", "Fira Code", "Cascadia Code", "Consolas", monospace`
  用于顶部终端标签（`>_`）、数据类型代号（如 `uint8`）、数值范围、输入框内容以及分组英文标题。
- **字号与字重梯度**：
  - **顶部终端标签**：`11.5px`，字重 `600`，字间距 `0.04em`
  - **页面主标题**：`22px`，字重 `650`，字间距 `-0.01em`
  - **副标题与工具栏按钮**：`13px`，字重 `400` ~ `500`
  - **表头与分组栏**：`11px` ~ `12px`，字重 `600`
  - **输入框数值**：`13.5px` 等宽字体，确保长整型与 64 位二进制清晰易读

## 4. 空间布局与栅格结构

- **版心与外边距**：
  - 容器最大宽度：`980px`，水平居中（`margin: 0 auto`）。
  - 页面内边距：桌面端 `48px 32px 64px`，移动端（`≤820px`）自适应收敛为 `24px 16px`。
- **顶栏结构（Header）**：
  - 左右两端对齐（`justify-content: space-between; align-items: flex-end`），底部留出 `20px` 内边距与 `28px` 外边距，并以 `1px solid var(--border-soft)` 分隔。
- **主面板四列栅格（Grid Row）**：
  - 列宽比例：`grid-template-columns: 175px 210px minmax(240px, 1fr) 235px`，列间距 `18px`。
  - 行内边距：上下 `13px`、左右 `24px`，兼顾同屏信息密度与视觉呼吸感。

## 5. 核心组件样式细节

- **直角分段切换器（Segmented Control）**：
  - 外层容器使用 `1px solid var(--border-strong)`，内部按钮之间通过 `border-right: 1px solid var(--border-strong)` 分隔。
  - 激活态按钮背景填充 `var(--accent-clay)`，文字为纯白 `#FFFFFF`，无阴影、无圆角。
- **分组分割条（Section Divider）**：
  - 采用 `#F8F6F0` 微暖底色，上下 `1px solid var(--border-soft)` 边框，内边距 `8px 24px`，使用等宽字体展示简短分类名（如 `INT · 有符号整型`）。
- **直角输入框与内嵌提示**：
  - 默认态：`background: #FCFBF9; border: 1px solid #CFCAC0; padding: 8px 34px 8px 12px`。
  - 聚焦态：背景变为纯白 `#FFFFFF`，边框变为 `#D97757`，外加 `0 0 0 2px rgba(217, 119, 87, 0.18)` 直角焦点光环。
  - 右侧内嵌极简 `↵` 符号，不额外包裹按钮边框，减少视觉噪音。
- **直角状态标签（Status Badge）**：
  - 内边距 `4px 10px`，采用语义浅底色 + 对应深色文字 + `1px solid` 同色系浅边框，仅在触发操作的当前行展示。

## 6. 供其他对话直接使用的 Prompt 模板

在其他对话中需要生成类似风格的网页或工具时，可直接复制以下提示词：

```markdown
请使用「Claude Code 浅色极简直角风格」编写单文件离线 HTML 页面，具体视觉与交互规范如下：
1. 全局直角几何：所有卡片、按钮、输入框、标签均设置 `border-radius: 0`，禁止使用圆角或厚重投影。
2. Claude Code 浅色配色：
   - 页面背景：`#FAF9F5`（暖米白），主卡片背景：`#FFFFFF`，次级表头/标签背景：`#F3F1EA`，分组栏背景：`#F8F6F0`
   - 边框颜色：主结构框与输入框 `#CFCAC0`，内部分割线 `#E5E2D8`
   - 强调色：陶土橙 `#D97757`（深色 `#BE5B3B`，浅底色 `#FDF3EF`，聚焦光环 `0 0 0 2px rgba(217, 119, 87, 0.18)`）
   - 文字颜色：主文字 `#141413`，次级文字 `#5C5A54`，弱化文字 `#8C887E`
   - 状态颜色：成功（文字 `#28663B` / 背景 `#EBF5EE` / 边框 `#C0E0C8`）、错误（文字 `#B83525` / 背景 `#FDECE9` / 边框 `#F3C2BA`）、警告/超范围（文字 `#9E4B24` / 背景 `#FDF2EB` / 边框 `#F2CFC0`）
3. 字体规范：UI 文字使用系统无衬线字体，代码标识、量程、输入框数值统一使用等宽字体（`JetBrains Mono`, `SF Mono`, `Consolas`, `monospace`）。
4. 适当留白与精简注释：版心最大宽度 `980px`，页面外边距 `48px 32px 64px`，数据行内边距 `13px 24px`；界面文案保持克制精炼，不堆砌多余的说明卡片或装饰性元素。
```

## 7. 基础 CSS 模板代码

```css
:root {
  --bg-canvas: #FAF9F5;
  --bg-surface: #FFFFFF;
  --bg-subtle: #F3F1EA;
  --bg-input: #FCFBF9;
  --bg-hover: #F7F5EE;
  --border-soft: #E5E2D8;
  --border-strong: #CFCAC0;
  --accent-clay: #D97757;
  --accent-clay-dark: #BE5B3B;
  --accent-clay-tint: #FDF3EF;
  --accent-focus-ring: rgba(217, 119, 87, 0.18);
  --text-primary: #141413;
  --text-secondary: #5C5A54;
  --text-muted: #8C887E;
  --success-text: #28663B;
  --success-bg: #EBF5EE;
  --success-border: #C0E0C8;
  --error-text: #B83525;
  --error-bg: #FDECE9;
  --error-border: #F3C2BA;
  --warn-text: #9E4B24;
  --warn-bg: #FDF2EB;
  --warn-border: #F2CFC0;
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Microsoft YaHei", sans-serif;
  --font-mono: "JetBrains Mono", "SF Mono", "Fira Code", "Consolas", monospace;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  border-radius: 0;
}

body {
  background-color: var(--bg-canvas);
  color: var(--text-primary);
  font-family: var(--font-sans);
  line-height: 1.5;
  padding: 48px 32px 64px;
  -webkit-font-smoothing: antialiased;
}
```
