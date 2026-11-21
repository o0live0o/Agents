# dotnet format 使用说明

`dotnet format` 是 .NET SDK 自带的代码格式化命令，按仓库中的 `.editorconfig` 规则改写代码，可替代手工调整缩进、换行与代码样式。

需要 .NET 6 及以上 SDK。默认在当前目录查找项目或解决方案文件，也可以显式指定。

## 基本用法

```bash
# 格式化当前目录下的解决方案或项目
dotnet format

# 指定项目或解决方案
dotnet format MyApp.csproj
dotnet format MyApp.sln
```

不带子命令时，会依次执行 `whitespace`、`style`、`analyzers` 三个阶段。

## 三个子命令

| 子命令 | 作用 | 依赖 |
|---|---|---|
| `whitespace` | 缩进、空格、换行、大括号位置等纯排版 | 无需额外配置 |
| `style` | 代码样式规则，如 `var` 使用、简化语法 | `.editorconfig` 中的 `csharp_style_*`、`dotnet_style_*` |
| `analyzers` | 分析器提供的修复建议，如 CA 系列规则 | `.editorconfig` 中的 `dotnet_diagnostic.*` |

```bash
# 只调整排版，不改变代码结构
dotnet format whitespace

# 只应用代码样式规则
dotnet format style

# 只应用分析器修复
dotnet format analyzers
```

只做排版时用 `whitespace`，改动范围最小、最安全；`style` 与 `analyzers` 可能修改代码结构。

## 只格式化指定文件

```bash
# 单个文件
dotnet format MyApp.csproj --include Program.cs

# 多个文件或目录，用空格分隔
dotnet format MyApp.csproj --include Program.cs Services/

# 反过来排除某些路径
dotnet format MyApp.csproj --exclude Migrations/ Generated/
```

`--include` 与 `--exclude` 的路径相对于项目或解决方案所在目录。

## 校验而不修改

```bash
# 存在不符合规范的代码时返回非零退出码，不写入任何文件
dotnet format --verify-no-changes

# CI 中常用：跳过还原，加快执行
dotnet format MyApp.sln --verify-no-changes --no-restore
```

返回值约定：`0` 表示已符合规范，`2` 表示需要格式化。适合放进流水线做门禁。

## 常用选项

| 选项 | 说明 |
|---|---|
| `--verify-no-changes` | 只检查不修改，有差异时返回非零 |
| `--include <路径>` | 只处理指定文件或目录 |
| `--exclude <路径>` | 跳过指定文件或目录 |
| `--no-restore` | 不执行隐式还原 |
| `--severity <级别>` | 只处理不低于该级别的诊断，取值 `info` / `warn` / `error`，默认 `warn` |
| `--diagnostics <ID>` | 只处理指定诊断 ID，可重复指定 |
| `--exclude-diagnostics <ID>` | 跳过指定诊断 ID |
| `--report <路径>` | 输出 JSON 格式的改动报告 |
| `--verbosity <级别>` | 日志详细程度，取值 `quiet` / `minimal` / `normal` / `detailed` / `diag` |

## 使用前提与常见问题

- `style` 和 `analyzers` 依赖 `.editorconfig`。若项目没有配置相应规则，这两个阶段不会产生改动，属正常现象。
- `--severity` 只对 `style` 与 `analyzers` 生效，`whitespace` 不受影响。
- 格式化结果与 IDE 不一致时，通常是 IDE 用了自己的设置而非 `.editorconfig`，以命令行结果为准。
- 首次运行需要还原 NuGet 包；离线或 CI 环境可先执行 `dotnet restore`，再配 `--no-restore`。
- 建议先 `git commit` 或 `git stash`，再运行 `dotnet format`，便于用 `git diff` 审阅改动。
