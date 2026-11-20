# Obsidian 知识库 协作约定（Codex / Claude 必读）

常浩的 Obsidian 知识库（`/Users/nat/Documents/Obsidian Vault`）的工程仓库：记这个项目的进度、交接、验收和以后的维护脚本，让 Claude、Codex、Hermes 都先按 Vault 了解常浩。**Vault 正文不在本仓库，也永远不进本仓库。** 进入顺序：本文件 → [CURRENT.md](CURRENT.md)（当前状态与唯一下一步）。需求以 Vault 里的项目卡 `10 项目/Obsidian 第二大脑改造.md` 为准（受限，只在本机读，不往本仓库抄）。所有面向用户的文字用简体中文。本文件只放怎么开始和做什么事之前读哪份；操作细节在 [docs/OPERATIONS.md](docs/OPERATIONS.md)，按需读对应一节。

## 两个仓库怎么分

| | Vault（`~/Documents/Obsidian Vault`） | 本仓库（`work/obsidian-kb`） |
|---|---|---|
| 放什么 | 知识正文：个人上下文、决定、精华、来源（含 GET NOTES） | 项目进度、交接、验收记录（只写结论和笔记路径）、维护脚本 |
| 远端 | 没有，只在本机；Vault AGENTS 禁止 `git push` | GitHub 私有仓库 `Natsukashii1997/obsidian-kb` |
| 改动规矩 | Vault 自己的 `AGENTS.md`（写入、确认、禁止项） | 本文件 +《工程流程》 |

依据：Vault 里的「决定/Vault 只开放给本人本地入口」（受限和没标注的内容不进外部服务）；常浩 2026-11-07 选「另建工程仓库」，Vault 正文照旧只在本机。

## 做什么事之前读哪份

| 要做的事 | 先读 |
|---|---|
| 改需求、加功能、跑验收 | Vault 项目卡 `10 项目/Obsidian 第二大脑改造.md`（本机读） |
| 往 Vault 里写、改、移动、删除 | Vault 的 `AGENTS.md`，按它的确认和禁止项做；Vault 的提交只在 Vault 里做 |
| 改检索工具 `vault-read` | 代码在 Vault 的 `.claude/scripts/vault_mcp.py`，改 `.claude/` 要常浩先确认（Vault AGENTS） |
| 改 Hermes 的 vault 技能 | 数字分身仓库 `hermes/skills/vault/` |
| 改 GET 笔记索引 | get-index 仓库（`work/get-index`，即本项目的 KB-04） |
| 排障、改脚本、装 launchd 或定时任务 | 先 `grep -n 关键词 docs/LESSONS.md`，再搜全局坑 `/Users/nat/Documents/ChatGPT/个人/work/agent-kit/LESSONS.md` |
| 开需求、拆任务、实现、审查 | 下面「Agent skills」一节 |
| 写交接、验收别人的交接 | [docs/templates/handoff.md](docs/templates/handoff.md) 骨架；[OPERATIONS](docs/OPERATIONS.md)「交接写法」「报告与验收」 |

项目长出接口、样式、发布等文档后，在这张表里加一行。

## 分工

- 以用户每轮明确授权的范围为准；授权外的不做。
- 一次只做一件授权的事；范围外的问题记进交接，不顺手修。
- 并行的会话各开独立 worktree，不在同一目录改代码；做不到时照全局坑 G-04，只提交自己的改动。

## 运行

```sh
node --test tests/*.test.mjs     # 交接格式 + 通用规则（含自测）
node scripts/check-rules.mjs     # 只跑通用规则检查
git config core.hooksPath scripts/hooks   # 装提交、推送前的必经检查（每个克隆、每台电脑各一次；不用 --no-verify 绕过）
```

## 结构

```
AGENTS.md            入口（本文件）
CURRENT.md           当前状态与唯一下一步；每轮交接写在顶部
docs/
  agents/            Matt Pocock 技能读的约定：issue 看板、分流标签、领域文档
  LESSONS.md         本项目的坑（排障、改脚本前先搜）
  OPERATIONS.md      操作手册：交接写法、报告与验收
  templates/handoff.md  交接一节的骨架
scripts/
  check-rules.mjs    通用规则检查（只查机器能判定的）
tests/               node:test 测试
```

## Agent skills

开发流程用 Matt Pocock skills（`~/.agents/skills/`）：大活走 `to-spec` → `to-tickets` → `implement` / `tdd` → `code-review` → PR；小改（一张 issue、一天内做完）跳过 `to-spec`。Claude 用 `/技能名`，Codex 用 `$技能名`；这几个技能只在点名时加载，不会自动触发。

### Issue tracker

需求、规格与任务用本仓库 GitHub Issues。见 `docs/agents/issue-tracker.md`。issue、PR、提交说明里同样只写结论和 Vault 笔记路径，不贴受限或没标注笔记的原文。

### Triage labels

五个默认分流标签。见 `docs/agents/triage-labels.md`。

### Domain docs

single-context：根目录 `GLOSSARY.md` 与 `docs/adr/`，用到时再建。见 `docs/agents/domain.md`。

改动怎么走、分支怎么起名、交接写在哪、怎么合并：见 `/Users/nat/Documents/ChatGPT/个人/work/agent-kit/工程流程.md`。

## 规则

- **Vault 内容不进本仓库**：不复制、不摘抄笔记正文、标题清单、索引数据或检索结果；交接和验收记录只写结论、数量和笔记路径（《工程流程》第 7 节）。拿不准某段算不算正文，按受限处理，不写。
- 验证用的备份、日志等证据放 `~/Library/Application Support/obsidian-kb/evidence/`，不放 `/tmp`（全局坑 G-01）；私人数据与完整日志不进仓库。
- 本机服务只监听 127.0.0.1；新增依赖先改 `scripts/check-rules.mjs` 的 `ALLOWED_DEPS`，并在交接里说明理由（`check-rules` 会查）。
- 页面上的时间经统一的格式化函数按北京时间显示，不直接截取 ISO 字符串（全局坑 G-02，`check-rules` 会查）。

## 交接格式

每轮完成后，在 CURRENT.md 顶部新增一节（旧节保留在下面，过长时归档到 `docs/history/`），骨架照抄 [docs/templates/handoff.md](docs/templates/handoff.md)；`tests/handoff-format.test.mjs` 会检查顶部一节有没有日期、「时间：开工」「测试：」「本轮新坑：」三行，新坑编号在 LESSONS 里要存在。每一行怎么写、报告与验收的防伪造五条见 [OPERATIONS](docs/OPERATIONS.md)「交接写法」「报告与验收」；没做就写「未做」。
