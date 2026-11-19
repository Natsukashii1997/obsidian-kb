# 当前状态

当前阶段目标：Vault 项目卡三条验收全部通过（第 3 条：Hermes 答 10 题命中 ≥ 8）。（2026-11-08 常浩确认，digital-twin#114）

更新：2026-11-07。知识库 11-05 重建完成；Vault 项目卡 `10 项目/Obsidian 第二大脑改造.md` 的三条验收：第 1 条 11-05 通过，第 2、3 条还没测（验收内容和结果表都在项目卡里，本仓库只记结论）。唯一下一步：常浩按项目卡第 3 条出题，跑一轮 Hermes 按库作答验收，结果记进项目卡验收表，这里补一行结论。

## LESSONS 升级行补层级词、加规则检查（2026-11-19，Claude，跨项目复盘收尾子 agent，常浩批了「老项目接入 LESSONS 升级行检查：一次补齐」；已提交到 `claude/lessons-level`、PR 待合并、无需发布）

- 时间：开工 `2026-11-19 10:18`（`date '+%F %H:%M'` 记录）。来由：跨项目复盘 D 批，各项目 LESSONS 的「升级」行统一带层级词，并由规则检查守住。
- 改了哪些文件：`scripts/check-rules.mjs`（加 `lessons` 规则：每条 `## L-xx` 要有以〔结构〕〔机检〕〔体检〕〔文字〕〔提醒〕开头的「- 升级：」一行，写法照起步骨架）；`tests/check-rules.test.mjs`（加 1 条测试：缺升级行、升级行不带层级词各报一次，格式样例和合规条目不误报）；`docs/LESSONS.md`（「怎么升级」补一条层级词要求，格式样例改成「〔提醒〕未升级。」，与起步骨架一致）；本仓库 LESSONS 还没有真实条目，规则在真实文件上为绿。另修起步骨架带来的漏报：判断占位标题前先去掉反引号里的内容（标题里的 `<all_urls>` 之类原来会让整条不查），写法同 chatgpt-clipper#10（`cd80285`），并加 1 条测试；骨架本身由主会话另行处理。本节交接。
- 测试：`node --test tests/*.test.mjs` 改前 `ℹ pass 4` / `ℹ fail 0`，改后 `ℹ pass 6` / `ℹ fail 0`；`node scripts/check-rules.mjs` 改前改后都输出 `通用规则检查通过`。
- 验证：在 worktree `~/Sandbox/worktrees/obsidian-kb-lessons-level` 里跑；改前改后日志在 `~/Library/Application Support/obsidian-kb/evidence/2026-11-19-lessons-level/`。
- 截图：无。
- 发布与回读：无需发布。
- 与授权不一致之处、遗留问题、范围外发现：规则检查没有接进 CI 或提交前钩子，只在手动跑测试时生效（照实记录，没为此改结构）。
- 本轮新坑：无。

## 项目起步：登记为活跃项目、另建工程仓库（2026-11-07，Claude，常浩说「登记为活跃，并建立 github 仓库」，选「另建工程仓库」；已提交并推送、无需发布）

- 时间：开工 `2026-11-07 20:22`（`date '+%F %H:%M'` 记录）。来由：常浩看全量项目盘点时发现知识库没算进项目；Vault 只在名册 inventory 里「只登记」。
- 改了哪些文件：新建本仓库，用「项目起步」铺骨架：`AGENTS.md`（两个仓库怎么分、Vault 内容不进本仓库）、`CURRENT.md`（本文件）、`docs/LESSONS.md`、`docs/OPERATIONS.md`、`docs/templates/handoff.md`、`docs/agents/` 三份、`tests/handoff-format.test.mjs`、`scripts/check-rules.mjs` 与 `tests/check-rules.test.mjs`、`.github/pull_request_template.md`。Vault 一个文件都没改。
- 测试：`node --test tests/*.test.mjs` 原样结果 `ℹ pass 4` / `ℹ fail 0`；`node scripts/check-rules.mjs` 输出 `通用规则检查通过`。
- 验证：推送后 `gh repo view` 回读为私有；`ops/check_repos.py` 回读仓库设置。
- 截图：无。
- 发布与回读：无需发布。
- 与授权不一致之处、遗留问题、范围外发现：
  - 为什么另建仓库：Vault 1248 个文件里 112 篇标受限、979 篇没标注（多是 GET NOTES），「决定/Vault 只开放给本人本地入口」规定这些不进外部服务，Vault AGENTS 也禁止 `git push`；常浩选另建工程仓库，Vault 正文照旧只在本机。
  - 知识库以前的三个旧方案仓库，11-07 按「停滞项目全部删除」移到了 `~/Archive/repos/待删/`：`Obsidian Second Brain`（Codex 10-09 的「持续知识成长升级」，没有提交，`staging/` 里 T1、T2 两批候选没发布进 Vault）、`knowledge-base`（llm-wiki 模式）、`brain`（GBrain）。11-05 的重建看上去已经取代它们，但 Second Brain 的候选有没有都进 Vault 没逐条核对。
- 本轮新坑：无。
