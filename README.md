# Ahsarah Survive Game

一个以沙漠城镇“阿萨拉”为舞台的 Agent 驱动生存叙事游戏。

当前目标不是立即构建完整的图形化开放世界，而是先通过 Discord 文字世界验证核心玩法：

- 每个 NPC 拥有独立人格、目标、关系、认知和记忆；
- NPC 只能依据自己感知到的信息行动；
- 玩家可以沟通、劝说和传播信息，但不能直接控制 NPC；
- 外部危机、资源约束与 NPC 私人动机共同推动涌现叙事；
- 世界状态由确定性规则维护，Agent 只能提交待校验的行动意图。

## 当前 Vertical Slice

首个场景暂定为《阿萨拉：噬日风暴》：

- 6 个 NPC；
- 6 个游戏回合；
- 1 个主广播频道和多个地点频道；
- 水、物资、秩序与风暴倒计时构成主要压力；
- 玩家和 NPC 需要维持水分、饱腹、体力和健康；
- 通过探索、采集、运输和设施生产形成有限的生存循环；
- 多条可组合的生存路线，不设置唯一正确结局。

## 设计文档

- [沙漠城镇 Vertical Slice](docs/game-design/desert-town-vertical-slice.md)
- [生存、采集与生产系统](docs/game-design/survival-production-system.md)
- [初始系统架构](docs/architecture/initial-architecture.md)
- [开发路线图与决策清单](docs/roadmap.md)

## 当前阶段

项目处于 Phase 0：设计基线。

下一步是确认玩家定位、回合节奏、信息可见范围和对话自由度，再定义领域类型与场景配置 schema，进入不依赖 Discord 和 LLM 的确定性模拟内核开发。
