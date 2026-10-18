# Project Status

一个用于编码 agent 的项目状态盘点 skill。在清理分支、worktree 或选择下一项任务前，先形成有证据的项目全景报告。

## 内容

- 当前 PR / MR 与 agent 进度。
- 最近完成的交付，默认检查最近五个已合并项目。
- 分支、worktree、未提交修改与清理候选。
- spec、issue 和文档的状态差异与更新建议。
- 下一步可选任务、依赖和建议。
- 自动 fetch 远端信息，报告本地修改、已有冲突和同步风险。
- 基于会话证据，提出值得写进文档或自动检查的经验。

## 安装

### Claude 插件安装

需要在本地安装 Git，并确保 Claude 所在环境能够访问本仓库。添加 marketplace 只会登记插件来源，不等于安装插件，也不会授予仓库访问权限。

在 Claude 界面中安装：

1. 打开插件的 **Add marketplace**。
2. 输入 `larryboiNEUQ/project-status` 并添加。
3. 点击 **Sync** 同步 marketplace。
4. 安装 `project-status` 插件。

在 Claude Code 中执行：

```text
/plugin marketplace add larryboiNEUQ/project-status
/plugin install project-status@larry-project-status
```

安装后，在目标项目中使用 `/project-status:project-status` 调用 skill。

### 其他 agent 宿主

将本仓库克隆到你的 agent 支持的 skill 目录中。例如，使用用户级 `.agents/skills` 目录的宿主：

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/larryboiNEUQ/project-status.git ~/.agents/skills/project-status
```

不同宿主的 skill 发现目录和调用方式可能不同。请按宿主文档调整安装位置。已有同名目录时，请先检查内容，不要直接覆盖。

## 使用

在目标项目中显式调用 `project-status` skill。例如，支持 slash command 的宿主可使用 `/project-status`；也可以让 agent 加载 `SKILL.md`，然后请求：

> 更新远端信息，盘点项目进度、本地修改和冲突风险，给出清理与复盘建议。不要合并代码、推送或修改文档。

报告先给结论，再列出“远端信息、本地修改和同步决定”模块，然后按五个部分输出：

1. 当前 PR 和 agent 进度。三个子表统一列出 PR 或任务、关联 spec 编码与名称、agent、当前状态或交付结果、证据、阻碍或下一步。
2. 分支和 worktree。
3. 待更新的 spec、issue 和文档。
4. 值得留下的复盘经验。分别用表格列出项目环境与流程经验、现有 skill 改进建议，包含优先级、经验、会话证据、改进建议、目标文件或配置和预期收益。
5. 项目下一步可以做的工作。

## 约束与依赖

- 自动 fetch 是盘点中唯一允许的项目状态变更：更新远端跟踪信息并下载 Git 对象。不 prune，不改工作文件、暂存区或本地分支。
- 不自动 pull、合并、rebase、stash 或 push，也不执行构建、测试、删除或 tracker 更新。
- 已有冲突与未来同步风险分开报告。分支分叉或文件改动重叠不等于已确认冲突；当前无冲突也不保证同步后无冲突。
- 是否同步本地代码、推送或处理冲突，由用户另行决定。
- 清理建议不等于删除授权。执行前须获得用户批准并重新检查证据。
- 优先使用项目已有的 Matt 配置；没有配置时，按项目文档发现信息源。
- 以下引用不包含在本仓库中；缺失时说明情况并按内置规则继续：
  - `ask-matt`：理解已有项目配置，推荐下一步流程。
  - `bro`：让报告清楚、简短、少用术语。只参考语言要求，不复述上一条消息。
  - `unslop`：去掉报告里的套话、空话和难懂表达，保留证据与不确定性。
  - `principle-prove-it-works`：直接检查实际文件、运行状态和远端结果，不只相信 agent 自述。不因此运行测试或创建核验文件。
  - `retro`：参考环境和流程改进分类，提出文档入口、检查配置等建议。
  - `reflect`：与 `retro` 并列，参考长期经验筛选、判断与工具使用等分析角度，将建议定位到实际读过的现有 skill。排除一次性问题和已有规则正确覆盖的经验。
  - 复盘默认只检查当前会话。两者只供分析参考，不自动启动评审 agent、创建任务、运行调优或修改文件。完整复盘流程可作为下一步选项。
  - `writing-for-agents`：通过 `retro` 间接引用，指导可长期保留的说明如何组织。
- Git、代码托管平台、tracker 和 agent runner 的可访问性决定核验范围。不可访问的信息须标记为未核验。
- `disable-model-invocation: true` 表示在支持此字段的宿主中，由用户显式调用，而非由模型自动触发。

完整执行规则见 [SKILL.md](SKILL.md)。
