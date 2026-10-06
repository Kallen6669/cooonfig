# Codex 配置备份

本目录保存本次调整的 Codex CLI 终端设置，和 [`../vscode/`](../vscode/) 中的配置配合使用。`config.toml` 是可以迁移的配置片段，不是本机完整配置的副本。

## 配置作用

```toml
[tui]
alternate_screen = "never"
```

Codex 使用普通滚动界面，保留终端原生文字选择。这与 VS Code 的 `terminal.integrated.rightClickBehavior = "copyPaste"` 配合，实现有选区时右键复制、没有选区时右键粘贴。

Shift+Enter 换行由 [`../vscode/keybindings.json`](../vscode/keybindings.json) 中的快捷键负责。它发送 `\u001b[13;2u` 来保留 Shift 修饰键；不要改为 `\n`，VS Code 的 `sendSequence` 会将换行字符转换为普通回车。

## 换机器时如何使用

1. 先按 [`../vscode/README.md`](../vscode/README.md) 将 VS Code 设置和快捷键合并到新机器的用户配置。
2. 找到运行 Codex 的环境对应的配置文件：

   | 运行环境 | 默认配置文件 |
   |---|---|
   | WSL、Linux、macOS | `~/.codex/config.toml` |
   | 原生 Windows | `%USERPROFILE%\.codex\config.toml` |
   | 自定义 `CODEX_HOME` | `$CODEX_HOME/config.toml` |

   Windows 上的 VS Code 配合 WSL Codex 时，两个 VS Code JSON 文件放在 Windows 用户配置目录；Codex 的 TOML 文件放在 WSL 用户目录。用户名可以与旧电脑不同。

3. 新环境没有配置文件时，可以创建 `.codex` 目录并复制本目录的 `config.toml`。已有配置时，先备份，再将 `alternate_screen = "never"` 合并到已有的 `[tui]` 节；没有该节时再添加，避免重复定义 `[tui]` 或覆盖其他配置。
4. 保存后重新启动 Codex。已运行的聊天可在任务结束后使用 `/exit` 退出，再用 `codex resume --last` 恢复。VS Code 设置通常自动刷新；未刷新时执行 `Developer: Reload Window`。

这些文件需要复制或合并到程序实际读取的位置；仅修改本仓库中的备份不会自动应用到 VS Code 或 Codex。

## 验证与维护

- 在 Codex 输入“第一行”，按 Shift+Enter，再输入“第二行”：应保留为两行草稿，按普通 Enter 才发送。
- 重新拖选终端输出后右键：应复制。取消选区后右键：应粘贴。
- 界面模式和换行序列已在 Codex CLI 0.160.0 的隔离终端中验证；新机器仍需检查真实键盘和鼠标操作。其他终端需要对应的按键与右键配置，VS Code JSON 不会控制 Windows Terminal 等独立程序。
- 后续通用 Codex 设置可以继续维护在这里；机器路径、登录凭据和聊天记录不随这个配置片段迁移。

官方参考：[Codex 配置](https://learn.chatgpt.com/docs/config-file/config-basic)、[Codex 配置项](https://learn.chatgpt.com/docs/config-file/config-reference)、[VS Code 按键序列](https://code.visualstudio.com/docs/terminal/advanced#_custom-sequence-keyboard-shortcuts)、[VS Code 右键行为](https://code.visualstudio.com/docs/terminal/basics#_right-click-behavior)。
