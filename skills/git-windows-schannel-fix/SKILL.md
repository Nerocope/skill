---
name: git-windows-schannel-fix
description: 解决 Windows 上 Git（WorkBuddy 内置 PortableGit / 用户自装 Git for Windows）直连
  GitHub HTTPS 时的各类可用性故障：schannel
  证书吊销检查失败（CRYPT_E_NO_REVOCATION_CHECK）、CredentialHelperSelector 弹窗、GCM/manager
  段错误、提交身份缺失、fetch 后远程跟踪分支不落盘的间歇性怪癖。当用户在 Windows 上报 git clone/fetch/push 的 TLS
  错误、凭据弹窗、rc=139 崩溃、Author identity unknown、或 origin/main 解析不到时使用。覆盖多 git
  共存（PortableGit / software git / 其他 agent 自带 git）时的安全配置方式。
agent_created: true
disable-model-invocation: true
---

# Git Windows（PortableGit）HTTPS 可用性修复

## 适用场景
- `git clone/fetch/push https://github.com/...` 报 `schannel: ... CRYPT_E_NO_REVOCATION_CHECK (0x80092012)`。
- 推送时反复弹 CredentialHelperSelector 选择器需手动选。
- `git-credential-manager` / `manager` 调用即段错误（rc=139）。
- `git commit` 报 `Author identity unknown` / `unable to auto-detect email`。
- `git fetch` 报 `[new branch]` 成功但 `git rev-parse origin/main` 仍 unknown、`branch -r` 空。

## 环境事实（本机实测）
- WorkBuddy 内置 PortableGit：`C:\Users\wnm\.workbuddy\binaries\PortableGit\versions\1.2.0\`（Bash 跑的就是它）。
- 用户自装 Git for Windows：`C:\wnm\software\it`（系统 gitconfig `helper=manager`、无 selector，正常使用不弹窗）。
- 多套 git 共用同一份 `~/.gitconfig`（全局），但各自 `etc/gitconfig` 独立（selector 不跨 git 共享）。
- 本机 GitHub 令牌存于 Windows 凭据管理器，由 wincred 静默读取；AI 不接触令牌明文。

## 修复 A：TLS 吊销检查（必须双配置）
根因：本机网络/代理连不上 CRL/OCSP，Git 默认 schannel 强制吊销检查而失败。属 TLS/环境问题，非 git 逻辑。
`git clone -c` 的临时参数在 PortableGit clone 传输层不被读取，必须写进仓库 config：

```bash
rm -rf <dir> && mkdir <dir> && cd <dir>
git init -q
git remote add origin <https-url>
git config http.schannelCheckRevoke false   # 关掉 schannel 吊销检查
git config http.sslBackend schannel         # 必须显式设 schannel；单设上一条 fetch 仍可能失败
git fetch origin                            # 此时应成功
```

**必须同时设两条，缺一仍可能报 revocation 错误。**

## 修复 B：凭据助手（wincred，禁用 manager）
PortableGit 系统 gitconfig 默认 `credential.helper=helper-selector`，每次推送弹选择器。
- `mingw64/bin/git-credential-wincred.exe`：存在且可用，读 Windows 凭据管理器令牌，静默认证。
- `mingw64/bin/git-credential-manager.exe`（manager/GCM）：存在但**段错误 rc=139**（.NET Framework 程序在 Git Bash pty 下 Access Violation），不可用。用户自装 git 的 GCM 版本较新不受影响。
- ⚠️ wincred 在 `mingw64/bin/`，不是 `libexec/git-core/`；只查后者会误判"wincred 不存在"。
- "勾了总是仍弹"真因：`credential.helper` 多值成链（系统先→全局后），系统 selector 排链首每次先执行；"总是"只写全局，剔不掉系统 selector。

**根治（多 git 共存，优先 scoped，勿全局重置误伤）**：
```bash
# 多 git 环境：只改目标 git 的系统层，不动全局
git config --system credential.helper wincred
```
- 全局空值重置 `credential.helper=""` 会丢弃其他 git 的系统助手；本机 HanaAgent 自带 git 只有 manager、无 wincred，对它重置会推送失败。故优先 scoped。
- 验证（不泄露令牌）：`git -c http.schannelCheckRevoke=false -c http.sslBackend=schannel ls-remote <url>` 返回引用且 EXIT=0，即 wincred 静默认证成功。
- 判据：以 `ls-remote`/真实 push 成功为准，不以"文件存在"为准。

## 修复 C：提交身份
全局 `~/.gitconfig` 的 `[user]` 段本机常为空，commit 必失败。设一次全局身份（跨 git 共享、无害）：
```bash
git config --global user.name Nerocope
git config --global user.email Nerocope@users.noreply.github.com
```
仓库级想用不同身份可单独覆盖，不影响全局。注意：身份=署名（公开），凭据=令牌（保密），两者无关。

## 修复 D：fetch 后 origin/main 不落盘（间歇性怪癖）
根因：git↔沙箱 FS 边界**间歇性**故障（时序/竞态），fetch 已把提交号写入 FETCH_HEAD，但更新 `.git/refs/remotes/origin/main` 这一步"写进去不持久可见"。**非权限封锁、非 TLS、非凭据**（已实锤：干净 fetch 能落盘、rebase 的 `.git/rebase-merge/` 能写）。
表现：`rev-parse origin/main` → unknown；`branch -r` → 空；FETCH_HEAD/reflog 正常。
修法（由轻到重）：
1. 最轻（推荐先试，不删任何东西）：`git fetch origin main && git update-ref refs/remotes/origin/main FETCH_HEAD`
2. 只删 `.git` 重建：`rm -rf .git && git init && remote add && fetch`（工作区文件原封不动，仅丢未推送本地分支）
3. 删整个目录重 clone（几乎不用）
日常 `git push` 不需要本地 `origin/main` 书签，故该怪癖对"提交+推送"基本无影响。普通 re-fetch 不能自愈；re-init 是"换干净目录重试、成功率较高"非 100% 保证。

## 澄清：rebase 不会弄丢 .git（反驳误判）
他 AI 称"rebase 在沙箱把 .git 弄丢 / 沙箱禁止写 .git 子目录"——实测**不成立**：冲突/无冲突 rebase 均正常、`.git` 完好、`.git/rebase-merge/` 可写可清、fetch 后 refs/remotes 能落盘。该 AI 真实成因是 rebase 失败后自己 `git init` 重建仓库，覆盖了旧 `.git/config`（丢身份与远端），随后 commit 报 unable to auto-detect email——根因是"重建仓库"非"rebase 丢 .git"。正确做法：`git fetch origin && git merge origin/main`（或 `git rebase origin/main`），勿照搬 `fetch main:refs/heads/remote_main + checkout -f` 绕法（`checkout -f` 丢未提交改动）。

## 不推荐的路（实测失败）
- `git -c http.sslBackend=openssl`：报 `unable to get local issuer certificate (20)`，自带 ca-bundle.crt 加载报 `error adding trust anchors`，放弃。
- 全局 `credential.helper=""` 重置：误伤只有 manager 的其他 git（见修复 B）。

## 操作注意（避免误触发杀软）
在 `C:\Users\wnm` 等家目录大树上跑宽泛 Grep（底层 ripgrep=`rg.exe`）会读到卡巴斯基隔离备份里的可疑脚本而弹窗。排查时先用 Bash `find` 定位，再对单文件 Grep；或加 `--iglob '!*.bat' --iglob '!*.exe' --iglob '!*.dll'` 排除。

## 验证
推送后用 GitHub 连接器 `get_file_contents` 确认文件落地，作端到端验证。
