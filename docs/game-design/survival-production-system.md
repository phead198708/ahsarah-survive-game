# 生存、采集与生产系统

> 状态：Draft  
> 范围：《阿萨拉：噬日风暴》Discord Vertical Slice  
> 目标：让 NPC 的自主决策能够真实改变生存结果，同时保持首版规则有限、可计算、可重放。

## 1. 核心循环

```mermaid
flowchart LR
    A["环境与身体消耗"] --> B["产生个人/城镇需求"]
    B --> C["探索与采集原料"]
    C --> D["运输与分配"]
    D --> E["设施生产物品"]
    E --> F["消费、装备或建设"]
    F --> A
```

这个循环必须服务于人物冲突和主线选择，而不是成为独立的刷材料系统。

典型冲突：

- 干净水可以饮用，也可以用于制药；
- 废金属可以修复母井，也可以制作遗迹探索工具；
- 纤维可以制作沙暴面罩，也可以制作撤离所需的绳索；
- 燃料可以净水、照明，也可以保留给风暴期间的避难所；
- NPC 可能因为私人目标、关系或恐惧拒绝交出物资。

## 2. 首版范围限制

首版最多包含：

- 4 个个人生存指标；
- 8 类基础原料；
- 8 类可生产物品；
- 5 个生产或储存设施；
- 8 条基础配方；
- 每个角色每回合 1 个主要动作；
- 不加入装备耐久、品质、随机词缀、复杂负重、市场浮动价格和多级科技树。

这些限制用于确保系统仍然可以通过纸面模拟和单元测试验证。

## 3. 个人生存状态

玩家与 NPC 使用相同的基础 Survival State：

| 指标 | 范围 | 作用 |
| --- | ---: | --- |
| Hydration | 0–100 | 口渴程度；沙漠中的最高优先级 |
| Satiety | 0–100 | 饱腹程度；影响长期行动能力 |
| Energy | 0–100 | 体力；移动、采集和生产会消耗 |
| Health | 0–100 | 身体健康；受脱水、饥饿、疾病和受伤影响 |

环境高温作为状态修正，不增加第五条常驻数值。

### 3.1 暂定每回合消耗

以下数值仅作为首轮平衡基线：

| 变化 | 默认值 |
| --- | ---: |
| 基础 Hydration | -20 |
| 基础 Satiety | -10 |
| 基础 Energy | -10 |
| 城外采集额外 Hydration | -10 |
| 重体力行动额外 Energy | -15 |
| Rest 恢复 Energy | +30 |
| 1 份 Clean Water | Hydration +40 |
| 1 份 Travel Ration | Satiety +35 |
| 1 份 Medicine | 根据伤病恢复 Health |

### 3.2 阈值效果

- 50–100：正常；
- 25–49：行动效率或产量降低；
- 1–24：进入 Critical，生存需要可覆盖原有计划；
- 0：产生持续 Health 伤害；
- Health 为 0：角色进入 incapacitated，不立即从故事中永久死亡。

首版默认平衡必须保证：在没有额外灾难时，角色不会在第三回合前仅因基础消耗失去行动能力。

## 4. 原料

| ID | 名称 | 主要来源 | 主要用途 |
| --- | --- | --- | --- |
| `brackish-water` | 苦咸水 | 母井、地下渗水 | 净化为饮水 |
| `raw-food` | 基础食材 | 商栈、失踪商队 | 制作旅行口粮 |
| `desert-herb` | 沙漠药草 | 北门沙丘 | 制作药品 |
| `dry-fiber` | 干纤维 | 灌木、废弃营地 | 绳索、面罩、维修 |
| `scrap-metal` | 废金属 | 商队残骸、地下遗迹 | 维修工具 |
| `fuel` | 燃料 | 商栈、商队、遗迹储藏 | 净水、照明、避难 |
| `cloth` | 布料 | 商栈、商队残骸 | 药品、面罩 |
| `stone` | 石料 | 北门沙丘、遗迹入口 | 加固避难所 |

原料均为整数 Item Stack。世界引擎不允许出现负库存或小数库存。

## 5. 生产物品与配方

| 成品 | 输入 | 设施 | 建议技能 | 主要用途 |
| --- | --- | --- | --- | --- |
| Clean Water ×2 | Brackish Water ×2 + Fuel ×1 | 母井水房 | watercraft | 饮用、制药、口粮 |
| Travel Ration ×2 | Raw Food ×2 + Clean Water ×1 | 商栈厨房 | survival | 撤离、救援、恢复 Satiety |
| Medicine ×1 | Desert Herb ×2 + Clean Water ×1 + Cloth ×1 | 医馆 | medicine | 治病与恢复 Health |
| Rope ×1 | Dry Fiber ×2 | 工匠院 | crafting | 探索、救援、撤离 |
| Repair Kit ×1 | Scrap Metal ×2 + Dry Fiber ×1 | 工匠院 | engineering | 修井、修设施 |
| Sand Mask ×1 | Cloth ×1 + Dry Fiber ×1 | 工匠院 | crafting | 降低风沙和城外消耗 |
| Shelter Kit ×1 | Stone ×2 + Rope ×1 + Repair Kit ×1 | 工匠院 | construction | 加固地表避难所 |
| Torch ×1 | Fuel ×1 + Dry Fiber ×1 | 工匠院 | crafting | 进入地下夜海 |

配方版本属于 Scenario Definition。Agent 只能选择已经解锁的 Recipe ID，不能通过自然语言临时发明配方。

## 6. 地点、资源节点和设施

| 地点 | 资源或设施 | 风险 |
| --- | --- | --- |
| 母井水房 | 苦咸水节点、净水设施 | 污染、机关故障 |
| 工匠院 | 通用制作台、维修设施 | 争夺废金属和燃料 |
| 医馆 | 药品制作台、治疗床 | 疾病传播、药品优先级 |
| 骆驼商栈 | 食物、布料、燃料仓库和厨房 | 私藏、盗窃、交易冲突 |
| 北门沙丘 | 药草、纤维、石料 | 高温、迷路、风暴 |
| 商队残骸 | 食物、布料、金属、燃料 | 需要探索后解锁，存在伤员 |
| 地下夜海 | 金属、渗水、古代设施 | 坍塌、黑暗、机关 |

### 6.1 Resource Node

每个资源节点定义：

```text
ResourceNode
  nodeId
  locationId
  itemId
  remainingQuantity
  regenerationPolicy
  requiredSkill
  requiredItem
  hazard
  visibility
```

首版节点默认有限，不做自动再生。这样六回合中的资源选择才有意义。

### 6.2 Production Station

```text
ProductionStation
  stationId
  locationId
  supportedRecipeIds
  operationalState
  capacityPerTurn
  reservedBy
```

设施故障、地点封锁或被占用时，对应配方不可用。

## 7. Inventory 与所有权

库存分为：

- 角色个人 Inventory；
- 地点公共 Storage；
- 私人或隐藏 Storage；
- 已被 Production Job 预留的 Reserved Stock。

首版使用简单容量：

- 每名角色默认携带 6 个单位；
- Sand Mask、Torch 等装备占用 1 个单位；
- 地点仓库不设重量上限，但有所有权和可见性；
- 超出携带量必须通过 `haul`、分配给其他角色或留在地点。

物品所有权影响动作：

- 取用公共库存可能合法；
- 取用私人库存需要同意、交易、征用或偷窃；
- Agent 知道某件物品存在，不代表它拥有使用权限；
- 隐藏库存必须先被感知或调查发现。

## 8. 新增动作

| 动作 | 说明 |
| --- | --- |
| `collect` | 从已知 Resource Node 获取原料 |
| `forage` | 在沙丘寻找草药、纤维或食物 |
| `scavenge` | 从残骸或遗迹获取材料 |
| `haul` | 在地点间运输物品 |
| `craft` | 使用已知配方和可用设施生产物品 |
| `consume` | 饮水、进食或使用药物 |
| `equip` | 装备面罩、火把等物品 |
| `transfer` | 赠送、分配或交付物品 |
| `reserve` | 为已确认的生产/建设计划预留库存 |
| `build` | 消耗成品推进避难所或路线项目 |

原有 `trade`、`steal`、`repair`、`rescue` 等动作继续存在。

## 9. 生产任务和并发结算

Agent 可以提交 Production Job：

```text
ProductionJob
  jobId
  actorId
  recipeId
  stationId
  requestedQuantity
  inputReservations
  intendedRecipients
  survivalPlanId
  status
```

统一结算顺序：

1. 校验角色位置、状态和 Energy；
2. 校验 Recipe 是否已知；
3. 校验设施可用性和容量；
4. 原子预留输入物品；
5. 解决多角色争夺同一库存或设施；
6. 消耗输入；
7. 根据规则、技能和固定 seed 计算结果；
8. 产生输出物品和 Domain Event；
9. 释放未使用的 Reservation。

同一回合新采集的原料默认在回合结算后才可用于生产。首版不支持“采集后立刻跨地点制作”的同回合无限链。

## 10. NPC 如何选择生存和生产行为

Agent Observation 应包含：

- 自己的 Survival State；
- 当前地点可见的物品、节点和设施；
- 自己知道的公共/私人库存；
- 当前可执行动作和配方；
- 城镇已公开的资源需求；
- 已承诺的 Production Job；
- 相关 NPC 的请求、关系和债务；
- 当前生存路线缺少的物品。

NPC 决策优先级不是固定脚本，但 Utility fallback 可参考：

```text
utility =
  selfSurvivalUrgency +
  protectedPersonUrgency +
  goalContribution +
  relationshipWeight +
  promisedObligation +
  skillEfficiency -
  travelCost -
  hazardRisk -
  moralCost
```

人格差异示例：

- 莱拉可能把最后一份 Clean Water 用于 Medicine；
- 法里德可能保留 Fuel，要求城镇承认他的控制权；
- 萨雅可能优先制作 Travel Ration 和 Sand Mask；
- 塔里克可能在脱水时仍坚持制作 Repair Kit；
- 纳蒂娅可能征用私人库存，从而降低 Order；
- 伊德里斯可能把 Torch 和 Rope 用于遗迹探索，而不是救援。

LLM 决定“想做什么以及为什么”，世界规则决定“能否做到和得到多少”。

## 11. 与四条生存路线的连接

| 生存路线 | 关键物品 |
| --- | --- |
| 修复母井 | Repair Kit、Clean Water、Fuel |
| 打开夜海 | Torch、Rope、Repair Kit、Medicine |
| 穿越北部沙丘 | Travel Ration、Clean Water、Sand Mask、Rope |
| 商队物资坚守 | Rescue supplies、Shelter Kit、Medicine、Fuel |

至少三条路线必须依赖两阶段以上的“采集 → 生产 → 使用”链条，不能仅靠找到单个剧情道具完成。

## 12. 回合中的生存阶段

回合增加 `Upkeep`：

1. 根据天气、位置、装备和上一回合行动扣除 Hydration、Satiety 和 Energy；
2. 对达到 Critical 或 0 的指标应用状态效果；
3. 处理疾病、伤势和设施持续效果；
4. 自动生成个人需要，但不自动消费他人或私人库存；
5. 将结果作为本回合 Observation 的一部分。

自动消费只允许用于角色自己明确预留的口粮。否则是否共享最后一份水必须由角色行动决定。

## 13. 事件

新增事件类型：

- `SurvivalNeedChanged`
- `CharacterBecameCritical`
- `ResourceCollected`
- `ResourceNodeDepleted`
- `ItemReserved`
- `ReservationReleased`
- `ItemTransferred`
- `ItemConsumed`
- `ItemCrafted`
- `CraftingFailed`
- `StationDamaged`
- `StationRestored`
- `ProjectProgressed`
- `CharacterIncapacitated`

所有库存和生存状态变更都必须来自这些已提交事件。

## 14. Fallback 行为

模型不可用时：

1. Critical 生存需要优先；
2. 若自己有可用消耗品，则 `consume`；
3. 若附近受保护角色处于 Critical 且有资源，则 `transfer`；
4. 若已承诺 Production Job 仍可执行，则继续；
5. 否则选择与技能最高匹配的公开需求；
6. 没有可执行任务则 `rest`。

Fallback 不得读取隐藏库存或未感知资源节点。

## 15. 验收标准

1. 任意动作后不存在负库存。
2. 同一 Item Stack 不会被两个并发任务重复消耗。
3. 采集、生产、消费和运输均可追溯到 Domain Event。
4. 相同 World Snapshot、Action Intent 和 seed 产生相同结果。
5. Agent 不能生产未定义物品或绕过 Recipe 输入。
6. Survival Upkeep 在重放中得到完全相同的状态。
7. 默认平衡下角色不会在第三回合前仅因基础消耗 incapacitated。
8. 至少三条生存路线需要有效生产链。
9. 至少一个 NPC 会因 Critical Need 合理中断原有计划。
10. 至少一个场景测试覆盖“同一原料的多个竞争用途”。
11. 四条生存路线在正确协作下均可完成，但不能在一局中无代价地全部完成。
12. 首版原料和成品种类不超过本文约定范围。
