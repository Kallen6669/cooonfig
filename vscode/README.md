# VSCode 配置备份

本目录备份了两个文件：

- `keybindings.json` —— 自定义快捷键
- `settings.json` —— 用户设置（含 Vim 相关配置）

---

## 一、在新 VSCode 中这两个文件该复制到哪

两个文件都属于 VSCode 的**用户（User）配置**，目标是用户配置目录 `.../Code/User/`：

### Windows（主要）

```
C:\Users\<你的用户名>\AppData\Roaming\Code\User\
```

即把文件复制成：

```
C:\Users\<你的用户名>\AppData\Roaming\Code\User\keybindings.json
C:\Users\<你的用户名>\AppData\Roaming\Code\User\settings.json
```

> `%APPDATA%` 就是 `C:\Users\<用户名>\AppData\Roaming`，所以也可以写成
> `%APPDATA%\Code\User\keybindings.json`。

### 其他系统（备用参考）

| 系统 | 用户配置目录 |
|------|--------------|
| Windows | `%APPDATA%\Code\User\` |
| macOS | `~/Library/Application Support/Code/User/` |
| Linux | `~/.config/Code/User/` |

### 最省事的打开方式

在 VSCode 里按 `Ctrl+Shift+P` 打开命令面板，执行：

- `Preferences: Open Keyboard Shortcuts (JSON)` → 打开的就是 `keybindings.json`
- `Preferences: Open User Settings (JSON)` → 打开的就是 `settings.json`

在打开的标签页上右键 → **Copy Path / 复制路径**，就能直接定位到该目录。

### 注意事项

1. **`settings.json` 建议合并，不要直接覆盖。** 如果新 VSCode 里已经有
   `remote.SSH.remotePlatform`、扩展相关等配置，直接覆盖会丢失。可以先备份原来的，
   再把本文件里的键合并进去。
2. **`keybindings.json` 可以直接整体覆盖**（它只是一个数组）。
3. 复制完成后一般**无需重启**，VSCode 会自动重新加载；若没生效，重启一次即可。

---

## 二、这两个文件里的配置都有什么作用

### 1. `keybindings.json`（快捷键）

> 说明：同一按键在「编辑器」和「终端」下可能是不同功能，用 `when` 条件区分。

#### 编辑器 / 通用

| 快捷键 | 命令 | 作用 |
|--------|------|------|
| `Ctrl+1` | `workbench.action.focusFirstEditorGroup` | 聚焦第 1 个编辑区 |
| `` Ctrl+` `` | `workbench.action.terminal.toggleTerminal` | 显示 / 隐藏终端面板（聚焦终端） |
| `Ctrl+2` | `workbench.view.explorer` | 打开 / 聚焦左侧「文件资源管理器」 |
| `Ctrl+3` | `workbench.action.focusNextGroup` | 只切换编辑区焦点（右移，到最右循环回第一个），**不移动文件** |
| `Ctrl+4` | `moveEditorToNextGroup` / `moveEditorToFirstGroup` | 把当前文件移到**右侧**编辑区并聚焦；到最右后循环回第一个 |
| `Shift+J` | `workbench.action.previousEditor` | 向左切换到**上一个打开的文件**（标签页） |
| `Shift+K` | `workbench.action.nextEditor` | 向右切换到**下一个打开的文件**（标签页） |
| `Ctrl+X` | `workbench.action.closeActiveEditor` | 关闭当前文件 |
| `Ctrl+Shift+X` | `workbench.action.reopenClosedEditor` | 还原（重新打开）上次关闭的文件 |

> 编辑区用 `Shift+J` / `Shift+K` 切文件；终端里 `Ctrl+H/J/K/L` 是输入行光标移动，`Alt+H/J/K/L` 是分屏 / 终端组切换。

#### 终端（仅在终端聚焦 `terminalFocus` 时生效）

| 快捷键 | 命令 | 作用 |
|--------|------|------|
| `Ctrl+Alt+J` | `terminal.scrollDown` | 终端内容向下滚动（模拟滚轮向下） |
| `Ctrl+Alt+K` | `terminal.scrollUp` | 终端内容向上滚动（模拟滚轮向上） |
| `Ctrl+Shift+J` | `terminal.resizePaneDown` | 向下拉动终端框 → **缩小**终端占比 |
| `Ctrl+Shift+K` | `terminal.resizePaneUp` | 向上拉动终端框 → **增大**终端占比 |
| `Ctrl+H` | `terminal.sendSequence`（`\u001b[D`） | 当前输入行光标**左移**（等效 `←`） |
| `Ctrl+J` | `terminal.sendSequence`（`\u001b[B`） | 当前输入行光标**下移**（等效 `↓`） |
| `Ctrl+K` | `terminal.sendSequence`（`\u001b[A`） | 当前输入行光标**上移**（等效 `↑`） |
| `Ctrl+L` | `terminal.sendSequence`（`\u001b[C`） | 当前输入行光标**右移**（等效 `→`） |
| `Ctrl+O` | `terminal.new` | 新建一个终端 |
| `Ctrl+P` | `terminal.split` | 左右切分当前终端 |
| `Ctrl+[` | `terminal.kill` | 关闭当前终端 |
| `Alt+K` | `terminal.focusPrevious` | 上移 → 上一个**终端组**（已切分的多个子终端算作一个整体） |
| `Alt+J` | `terminal.focusNext` | 下移 → 下一个**终端组**（整体切换） |
| `Alt+H` | `terminal.focusPreviousPane` | 左移 → 当前组内**上一个子终端分屏** |
| `Alt+L` | `terminal.focusNextPane` | 右移 → 当前组内**下一个子终端分屏** |

> 层级区别：**`Alt+J` / `Alt+K`（下/上）** 在独立终端（标签/组）之间整体切换；**`Alt+H` / `Alt+L`（左/右）** 在同一个终端内部的分屏之间切换。

#### 已知冲突提醒

- **编辑区 `Shift+J` / `Shift+K` 会覆盖 VSCodeVim 的 `J`（合并行）、`K`（查看关键字）**，并且可能影响在插入模式下输入大写 `J` / `K`；若不希望这样，可改为 `Alt+J` / `Alt+K`。
- `Ctrl+X` 原本是编辑器的「剪切」，现在改为关闭文件（用 Vim 的 `x`/`d` 剪切可忽略此影响）。
- `Ctrl+[` 在终端里原本等价于发送 `Esc`，现在改为关闭终端，终端内的 vim/tmux 将收不到该 `Esc`。
- `Ctrl+Shift+X` 原本是「显示扩展」，已被覆盖。
- **`Alt+H/J/K/L` 覆盖了 shell 的 Meta 键绑定**：例如 `Alt+H`（向后删除单词）、`Alt+L`（单词转小写）等 readline 默认行为会被改掉。若某些终端把 Alt 组合吞掉导致无效，请检查终端的 Alt/Meta 设置。
- **终端 `Ctrl+H/J/K/L` 覆盖了 shell 的默认行为**：`Ctrl+H`（退格）、`Ctrl+J`（换行/回车）、`Ctrl+K`（删除到行尾）、`Ctrl+L`（清屏）都会变成光标移动；并且 `Ctrl+J/K`（下/上）在多数 shell 中即方向键，会触发**历史命令**浏览，而不是在当前输入行内垂直移动。

### 2. `settings.json`（用户设置）

| 配置项 | 值 | 作用 |
|--------|-----|------|
| `js/ts.locale` | `"zh-CN"` | JS/TS 相关提示使用中文 |
| `vim.insertModeKeyBindings` | `j k` → `<Esc>` | VSCodeVim：在插入模式下连按 `j`、`k` 退回到普通模式（代替 Esc） |
| `vim.normalModeKeyBindingsNonRecursive` | `J`、`K` → 空动作 | 屏蔽 VSCodeVim 的 `J`（合并行）和 `K`（查看关键字），避免与 `Shift+J/K` 切文件冲突 |
| `vim.handleKeys` | `"<C-a>": false` 等 | 不把这些 Ctrl 组合交给 Vim 处理，交还 VSCode：`Ctrl+A` 全选、`Ctrl+C` 复制、`Ctrl+V` 粘贴、`Ctrl+X` 剪切、`Ctrl+S` 保存、`Ctrl+Z` 撤销 |
| `vim.useCtrlKeys` | `false` | Vim 不接管任何 Ctrl 组合键，全部交给 VSCode（保证上面的自定义 Ctrl 快捷键生效） |
| `RainbowBrackets.depreciation-notice` | `false` | 关闭 Rainbow Brackets 插件的弃用提示 |
| `security.workspace.trust.untrustedFiles` | `"open"` | 未受信任的工作区中的文件仍可直接打开 |

> `settings.json` 依赖扩展：`vim`（VSCodeVim）、`Rainbow Brackets`。新环境需先安装这些扩展，相关配置才会生效。
