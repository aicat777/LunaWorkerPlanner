# Luna Worker Planner

Luna Worker Planner 是一个面向 Codex 的项目规划与执行协调插件。

它让当前对话中的高级模型担任项目规划师，将项目检查、代码修改、测试和其他具体工作委派给 `luna_worker` 子代理。这样可以把高级模型的能力集中在目标拆解、架构判断、风险分析和结果验收上，同时使用更轻量的工作代理完成边界明确的执行任务。

## 工作方式

- 规划师负责理解用户目标、确定范围、拆分任务和控制项目方向。
- 需要读取项目、验证状态、运行测试或修改文件时，由规划师调用 `luna_worker`。
- `luna_worker` 默认使用 `gpt-5.6-luna`，推理强度为 `max`。
- 对代码库任务，规划师会先让一个子代理在项目根目录创建或刷新 `.luna-worker-context.md`。
- 该文件只包含两部分：排除或折叠依赖、缓存、生成物等高容量目录后的项目文件树，以及按相对路径去重的文件解释。
- 后续子代理会先读取该文件。任务结束后，它只更新受影响的树节点，并为本次实际检查、创建或修改的文件新增或修订一至三句说明。
- 文件说明记录文件用途、重要内容或行为以及相关文件关系；不记录任务历史、命令和测试输出、项目目标、进度、决策、风险、架构总结或构建说明。
- 规划师只保留上下文文件路径和初始化状态，不在父对话中读取、复制或转发正文。
- 子代理运行期间，规划师等待最终结果，不与其重复执行同一项工作。
- 规划师不会简单转述子代理报告，而会分析其证据、推理、覆盖范围、风险和遗漏。
- 如果关键证据不足，规划师会委派一个更具体的核验任务，而不是亲自重新检查项目。
- 最终项目判断和后续计划由规划师负责。

## 适用场景

适合需要持续规划和分阶段执行的项目，例如：

- 理解现有项目并制定改造方案
- 将大型需求拆分为可执行的小任务
- 委派代码修改、测试和定向调查
- 检查执行结果是否满足项目目标
- 根据新结果持续调整里程碑和优先级

普通问答或仅依赖用户现有材料的规划讨论不需要强制调用子代理。

## 安装

添加 marketplace：

```text
codex plugin marketplace add https://github.com/aicat777/LunaWorkerPlanner.git
```

安装插件：

```text
codex plugin add luna-worker-planner@luna-worker-planner
```

将仓库中的：

```text
agents/luna-worker.toml
```

放入当前设备的：

```text
~/.codex/agents/luna-worker.toml
```

安装完成后，重新打开一个 Codex 对话并选择：

```text
@Luna Worker Planner
```

之后直接描述项目目标即可，不需要手动输入调用子代理的固定提示词。

## 更新

```text
codex plugin marketplace upgrade luna-worker-planner
codex plugin add luna-worker-planner@luna-worker-planner
```
