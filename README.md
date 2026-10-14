# Project Status

一个用于编码 agent 的项目状态盘点 skill。在清理分支、worktree 或选择下一项任务前，先形成有证据的项目全景报告。

## 内容

- 当前 PR / MR 与 agent 进度。
- 最近完成的交付，默认检查最近五个已合并项目。
- 分支、worktree、未提交修改与清理候选。
- spec、issue 和文档的状态差异与更新建议。
- 下一步可选任务、依赖和建议。

## 安装

将本仓库克隆到你的 agent 支持的 skill 目录中。例如，使用用户级 `.agents/skills` 目录的宿主：

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/larryboiNEUQ/project-status.git ~/.agents/skills/project-status
```

不同宿主的 skill 发现目录和调用方式可能不同。请按宿主文档调整安装位置。已有同名目录时，请先检查内容，不要直接覆盖。

## 使用

在目标项目中显式调用 `project-status` skill。例如，支持 slash command 的宿主可使用 `/project-status`；也可以让 agent 加载 `SKILL.md`，然后请求：

> 盘点这个项目的 PR、agent、分支、worktree 和文档状态，给出清理候选和下一步选项。不要执行清理或更新。

报告使用用户的语言，按四个部分输出：

1. 当前 PR 和 agent 进度。
2. 分支和 worktree。
3. 待更新的 spec、issue 和文档。
4. 项目下一步可以做的工作。

## 约束与依赖

- 盘点过程只读，不执行 fetch、构建、测试、删除或 tracker 更新。
- 清理建议不等于删除授权。执行前须获得用户批准并重新检查证据。
- 优先使用项目已有的 Matt 配置；没有配置时，按项目文档发现信息源。
- `ask-matt` 是可选协作 skill，不包含在本仓库中。不可用时，报告缺口并遵循项目文档流程。
- Git、代码托管平台、tracker 和 agent runner 的可访问性决定核验范围。不可访问的信息须标记为未核验。
- `disable-model-invocation: true` 表示在支持此字段的宿主中，由用户显式调用，而非由模型自动触发。

完整执行规则见 [SKILL.md](SKILL.md)。
