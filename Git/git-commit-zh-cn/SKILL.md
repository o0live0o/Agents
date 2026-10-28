---
name: git-commit-zh-cn
description: Conventional Commits 1.0.0 提交信息规范：格式、类型、破坏性变更与语义化版本映射。当用户需要编写或审查 commit message、决定版本号递增（PATCH/MINOR/MAJOR）时使用。
---

# Conventional Commits

## 提交信息结构

```
<type>[可选 scope]: <description>

[可选 body]

[可选 footer(s)]
```

- 一行开头是类型（如 `feat`、`fix` 这样的名词），紧跟可选 scope、可选 `!`，然后是必需的冒号加空格。
- description 紧跟在冒号加空格之后，用一句话概括改动。
- body 与 description 之间空一行，可多段，说明动机和影响。
- footer 可以有一个或多个，与 body 之间空一行。每个 footer 由单词 token、分隔符（`: ` 或 ` #`）和值组成；token 中的空格用 `-` 代替（如 `Reviewed-by`），`BREAKING CHANGE` 是唯一例外。

## 类型与版本映射

- `feat`：新增功能，对应 MINOR。
- `fix`：修复缺陷，对应 PATCH。
- 任何类型带 `!` 或 `BREAKING CHANGE:` footer，都对应 MAJOR。
- `build`、`chore`、`ci`、`docs`、`style`、`refactor`、`perf`、`test` 等其他类型也可以使用，它们自身不改变语义版本。
- 一个提交同时符合多个类型时，尽量拆成多个提交。

## 破坏性变更

- 破坏性变更必须标注在 type/scope 前缀或 footer 中，不能隐式存在。
- 在前缀中标注时，`!` 紧贴冒号之前，如 `feat!:`、`feat(api)!:`。
- 在 footer 中标注时，写作 `BREAKING CHANGE: <说明>`，必须全部大写。
- 两种写法可同时使用；用了 `!` 时可以省略 footer 的 `BREAKING CHANGE:`，此时 description 需说明破坏性变更。
- `BREAKING-CHANGE` 与 `BREAKING CHANGE` 作为 footer token 等价。

## 其他硬性规则

- scope 是描述代码库某一部分的名词，用括号包裹，如 `fix(parser):`。
- 除 `BREAKING CHANGE` 必须大写外，其余内容不区分大小写；大小写写法任选一种，同一仓库内保持一致。
- footer 的值可含空格和换行；出现下一个合法 token 加分隔符时，即视为新的 footer。
- 回滚提交用 `revert` 类型，并在 footer 用 `Refs:` 列出被回滚的 SHA。

## 示例

### 带 description 和破坏性变更 footer 的提交信息

```
feat: allow provided config object to extend other configs

BREAKING CHANGE: `extends` key in config file is now used for extending other config files
```

### 用 `!` 提示破坏性变更的提交信息

```
feat!: send an email to the customer when a product is shipped
```

### 带 scope 和 `!` 的提交信息

```
feat(api)!: send an email to the customer when a product is shipped
```

### 同时带 `!` 和 BREAKING CHANGE footer 的提交信息

```
feat!: drop support for Node 6

BREAKING CHANGE: use JavaScript features not available in Node 6.
```

### 无 body 的提交信息

```
docs: correct spelling of CHANGELOG
```

### 带 scope 的提交信息

```
feat(lang): add Polish language
```

### 多段 body 和多个 footer 的提交信息

```
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are
obsolete now.

Reviewed-by: Z
Refs: #123
```

### revert 提交

```
revert: let us never again speak of the noodle incident

Refs: 676104e, a215868
```

## 修正错误提交

- 合并或发布之前发现提交信息有误，用 `git rebase -i` 编辑历史。已经发布后，清理方式取决于所用工具和流程。

## 默认策略（不在规范内）

以下为项目偏好，不属于 Conventional Commits 规范；用户或团队另有约定时以其为准。

- 默认用 footer 的 `BREAKING CHANGE:` 记录破坏性变更；用户明确要求时才改用 `!`。
- 默认使用这些类型：`feat`、`fix`、`docs`、`refactor`、`test`、`chore`。
