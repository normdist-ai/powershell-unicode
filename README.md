# powershell-unicode

> CherryClaw Skill — 自动为 Windows 上的 PowerShell 命令注入 UTF-8 编码设置

[![CherryClaw Skill](https://img.shields.io/badge/CherryClaw-Skill-blue)](https://github.com/normdist-ai)

English | [中文](#中文说明)

---

## Problem

On Windows with Chinese/CJK locale, the system default code page is **GBK (CP936)**. PowerShell outputs text using this code page by default, but terminal tools like Git Bash expect **UTF-8**. This mismatch causes Chinese and other non-ASCII characters to appear as garbled text (mojibake / 乱码) in the output.

## Solution

This skill automatically prepends a UTF-8 encoding preamble to every PowerShell command executed via the Bash tool on Windows:

```
[Console]::OutputEncoding=[Text.Encoding]::UTF8;
```

## Install

In CherryClaw / CherryStudio, run:

```
/install normdist-ai/powershell-unicode
```

## Usage

Once installed, the skill is **fully automatic**. No manual steps needed.

When the agent executes a PowerShell command on Windows:

```bash
# Before (garbled Chinese output) ❌
powershell -Command "Get-ChildItem C:\用户"

# After (clean UTF-8 output) ✅
powershell -Command "[Console]::OutputEncoding=[Text.Encoding]::UTF8; Get-ChildItem C:\用户"
```

### Supported Scenarios

| Scenario | How it works |
|---|---|
| `powershell -Command "..."` | Prepends encoding preamble inside `-Command` |
| `powershell -NoProfile -Command "..."` | Same — preamble injected regardless of profile |
| `powershell -File script.ps1` | Switches to `-Command` mode and injects preamble |
| Scripts with `$` variables | Uses `-File` with preamble written into the `.ps1` file |

## How It Works

The skill's `SKILL.md` defines a set of rules that the agent follows whenever it's about to run PowerShell on Windows:

1. **Detects** the Windows platform (`platform: win32`)
2. **Injects** `[Console]::OutputEncoding=[Text.Encoding]::UTF8;` at the beginning of the `-Command` argument
3. **Handles edge cases** — `-File` mode, Bash `$` escaping, double-injection prevention

## Compatibility

- **OS**: Windows only (Linux/macOS PowerShell Core already defaults to UTF-8)
- **Agent**: CherryClaw, CherryStudio, or any Claude Code agent that uses the Bash tool on Windows
- **Locales**: Especially useful for Chinese (GBK/CP936), Japanese, Korean, and other non-UTF8 code pages

## License

MIT

---

<a id="中文说明"></a>

## 中文说明

### 问题

在中文 Windows 系统上，默认代码页是 GBK（CP936）。PowerShell 默认使用这个代码页输出文本，但 Git Bash 等 Bash 工具期望 UTF-8 编码。这种不匹配导致中文和其他非 ASCII 字符在输出中出现乱码。

### 解决方案

本技能会自动在每个通过 Bash 工具执行的 PowerShell 命令前注入 UTF-8 编码设置，确保中文正确显示。

### 安装

在 CherryClaw / CherryStudio 中运行：

```
/install normdist-ai/powershell-unicode
```

安装后**全自动生效**，无需任何手动操作。
