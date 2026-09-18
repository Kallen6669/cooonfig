# conf —— 个人编辑器配置备份

用于集中存放并版本管理个人常用的编辑器配置，方便在新机器 / 新环境上快速复用。

目前包含 **VSCode** 的配置，后续计划加入 **Neovim** 的配置。

---

## 目录结构

```
conf/
├── README.md              # 本文件：项目总览
├── vscode/                # VSCode 配置备份
│   ├── README.md          #   ↳ 详细的安装与说明（重点看这个）
│   ├── keybindings.json   #   ↳ 自定义快捷键
│   └── settings.json      #   ↳ 用户设置（含 VSCodeVim）
└── tests/                 # 临时测试文件（可忽略）
```

---

## 内容说明

### `vscode/`

备份的是 VSCode 的**用户（User）级配置**，两个文件分别是：

| 文件 | 对应 VSCode 功能 |
|------|------------------|
| `keybindings.json` | 自定义快捷键 |
| `settings.json` | 用户设置（含 VSCodeVim、Rainbow Brackets 等） |

配置整体围绕 **Vim 操作习惯 + 终端/编辑区快速切换** 设计，完整的按键表、作用说明、
已知冲突提醒以及「如何复制到新环境」，请见 **[`vscode/README.md`](./vscode/README.md)**。

**依赖扩展**（新环境需先安装，相关设置才生效）：

- `vim`（VSCodeVim）
- `Rainbow Brackets`

### `tests/`

临时的测试 / 试验文件，不属于实际配置，可以忽略或清理。

---

## 快速使用

### 1. 克隆仓库

```bash
# SSH（推荐，需已配置好 GitHub 的 SSH key）
git clone git@github.com:Kallen6669/cooonfig.git

# 或 HTTPS
git clone https://github.com/Kallen6669/cooonfig.git
```

### 2. 复制到本机 VSCode 用户配置目录

| 系统 | 用户配置目录 |
|------|--------------|
| Windows | `%APPDATA%\Code\User\`（即 `C:\Users\<用户名>\AppData\Roaming\Code\User\`） |
| macOS | `~/Library/Application Support/Code/User/` |
| Linux | `~/.config/Code/User/` |

把 `vscode/keybindings.json` 和 `vscode/settings.json` 复制进去即可。

> 最省事的定位方式：VSCode 里 `Ctrl+Shift+P` →
> `Preferences: Open Keyboard Shortcuts (JSON)` / `Preferences: Open User Settings (JSON)`，
> 在打开的标签页上右键 → **Copy Path**。

### 3. 注意事项

- `settings.json` **建议合并，不要直接覆盖**，否则可能丢失新环境已有的配置
  （如 `remote.SSH.remotePlatform`、扩展相关项等）。
- `keybindings.json` 是纯数组，可以直接整体覆盖。
- 复制后一般无需重启，VSCode 会自动重载；未生效则重启一次。

更多细节见 [`vscode/README.md`](./vscode/README.md)。

---

## 更新与维护

在本地改好配置后，把改动同步回本仓库并提交：

```bash
git add -A
git commit -m "update vscode settings"
git push
```

---

## TODO / 计划

- [ ] 加入 Neovim 配置（`nvim/`）
- [ ] 清理 `tests/` 下的临时文件
