# skill-refinement（技能迭代）

Subagent 学到了你的系统会遗忘的东西。

`skill-refinement` 是一套审核循环：把 subagent 的真实失败、反复成本和边界案例，转化为经过审核的、对可复用 skill 和 role brief 的改进。

重点不是"记更多"，而是"决定什么值得成为指令"。

无需 CLI。协议跑在文本文件和审核上：让 agent 读这份 README，把 lesson inbox 拷贝到你的工作区，每次边界任务结束后追加一条 lesson candidate。

---

如果你用 AI agent 做开发，这个仓库是给你的。

这个循环只有在 subagent 做**边界清晰、重复性高的工作**时才会转起来。一个 subagent 反复执行同一类任务——code review、migration、research、test harness。这种重复才会产出教训。一个主 agent 处理一次性的决策，信号太杂，无法产生可靠经验。

```text
🤖 subagent 执行边界重复任务
        ↓
📝 从真实失败/成本中浮现 lesson candidate
        ↓
📬 lesson 等待审核
        ↓
🚪 审核关卡决定什么可以泛化
        ↓
✅ 通过审核的 lesson 更新 skill 或 role brief
        ↓
🤖 下一个 subagent 加载改进后的方法
```

一个最小 lesson candidate 长这样：

```md
Task: 数据库迁移审核
Observed: 迁移在本地通过，但没有检查目标表是否有活跃的批量写入
Candidate rule: 迁移前，检查目标表上是否有活跃的长连接写入者
Decision: 已提升到 migration skill 中
```

只凭常识写出来的 skill 是浅的。从真实失败中长出来的 skill 不是。

## 适用任何 Agent

不需要框架、运行时或包安装。这是一套 agent 原生的工作约定。

1. 把 `templates/LESSON_INBOX.md` 拷贝到你的 agent 工作区。
2. 让你的 agent 阅读这份 README。
3. 用 role brief 和 skill 来做边界重复工作。
4. 要求 agent 每次任务结束后追加一条 lesson candidate。
5. 修改任何 skill 或 role brief 前，先审核 inbox。
6. 只提升有证据、可复现、有用的 lesson。
7. 下次加载改进后的 skill 或 role brief。

文本文件是部署面。审核是安全机制。

## 为什么有效

一个 skill 是一个假说。它说："这样做会产生好结果。"

真实工作是检验。当 agent 按照 skill 执行，出现崩溃、漂移或成本超出预期时，那就是数据。skill 错了，或不完整，或在比预期更窄的范围内才成立。

这个迭代循环认真对待这些数据：

```
💡 提出假说   →   用你当前最好的理解写出 skill
        ↑
        |
🔁 重复        ←   这个框架还成立吗？
        |          是 → 小更新     否 → 从零重新思考
        ↓
🔨 检验        →   让 agent 在真实任务上使用
        ↓
📋 收集        →   什么有效，什么失败，什么意外
        ↓
✏️  更新        →   把 lesson 提升到 skill 中
```

## 解决了什么问题

没有审核循环，skill 更新只会以两种方式失败：

**没有更新路径。** 同样的错误反复出现。同样的边界案例反复让 agent 踩坑。经验蒸发。

**直接自我更新。** Agent 自己改写规则。本地的偶发事故变成永久教条。skill 以另一种方式偏离现实。

lesson inbox 坐在这两种失败模式之间。教训从真实工作中积累。人和有能力的审核者决定什么值得泛化。只有通过审核的 lesson 才变成持久规则。

## 三层结构

1. **Role brief** — agent 在这个任务中是谁，它拥有什么，绝不能做什么，以及如何汇报。
2. **Skill** — 如何执行一类可复用的工作：方法、验证习惯、证据标准。
3. **Lesson inbox** — 一个低门槛的队列，在 lesson 被提升为持久规则之前暂存。

Subagent 使用 skill 并追加 lesson candidate。它们不拥有 skill 的更新权。那道门属于人类操作者——如果你有主 agent 工作流也可以交给它。Subagent 的工作是干活并报告它学到了什么。提升是别人的决定。

## 提升后的 Lesson 长什么样

实践中，被提升的 lesson 会变成 role brief 或 skill 底部的**本地知识区**——一组只有真正跑过这个项目的人才知道写的规则。

来自真实生产项目的例子：

> *迁移前，检查目标表上是否有活跃的长连接写入者。迁移返回 0 但批量任务正在写入，会让回滚路径未被测试。*

这条规则不在最初的 skill 里。它来自一次真实失败。它被审核、提升，现在每个接触该项目迁移的 agent 都会加载它。

## 两种模式

**手动 inbox** — lesson 进入 `LESSON_INBOX.md`，人类审核后直接编辑 skill。适合 skill 尚未版本控制、或规则风险较高的项目。

**Git 原生** — skill 变更以 commit 或 PR 的形式提交，由你或你信任的 agent 执行。人类审核 diff。merge 即提升，close 即拒绝。Git 历史就是审计记录。`LESSON_INBOX.md` 变为可选。

大多数项目从手动 inbox 起步，朝向 git 原生迁移。

## 仓库内容

先读 `docs/`。从 `templates/` 拷贝到你的工作区。

- `docs/CONCEPTS.md` — 术语定义
- `docs/WORKFLOW.md` — 操作循环
- `docs/MODES.md` — 手动 inbox vs git 原生，含决策表
- `docs/PROMOTION_CRITERIA.md` — 提升决策的审核问题
- `templates/LESSON_INBOX.md` — 初始 inbox
- `templates/worker_roles/` — role brief 模板
- `templates/skills/` — skill 模板
- `examples/migration-skill-iteration/` — 完整迭代周期，v1 到 v2
- `examples/mature-role-brief/` — 经过多次提升后的 role brief 实例
- `examples/mature-skill/` — 经过多次提升后的 skill 实例
- `examples/bad-promotion-example/` — 不应被提升的 lesson 示例

## 不是这些

- 不是自动 skill 更新器。审核关卡才是重点。
- 没有做边界重复工作的 subagent 就没有用。重复才是信号源。
- 不是人类判断的替代品。
- 不是 CLI 优先工具。是文本优先的协议，任何有能力的 agent 都能读和执行。

## 讨论

首次公开说明：https://x.com/HomuraTokido/status/2052946438288802256

相关架构讨论：https://github.com/NousResearch/hermes-agent/issues/21303

## License

MIT. 详见 `LICENSE`。
