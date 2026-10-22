<a id="top"></a>

# Super Company / 超级公司

面向 Victoria 3 1.13 的高强度公司模组：8 家玩家专属公司，以及 5 个可选择地区的年度开发项目。

An intentionally overpowered company mod for Victoria 3 1.13, with eight player-only companies and five annual development projects with state selection.

**[简体中文](#zh-cn) · [English](#english)**

<a id="zh-cn"></a>

## 简体中文

[概览与安装](#zh-overview) · [公司效果详解](#zh-companies) · [产业链](#zh-chains) · [年度事件](#zh-events) · [兼容性与实现](#zh-compatibility) · [游戏内验收](#zh-validation)

<a id="zh-overview"></a>

### 概览与安装

- 模组版本：2.0.0；目标游戏版本：Victoria 3 1.13.11，元数据使用 `1.13.*`。
- 八家公司覆盖资源、基础工业、建设、消费、军工、能源、科研与金融，保留明显超模的强度。
- 核心产业多数获得 +100% 至 +250% 吞吐量；当前脚本另为粮食农场提供 +500%、建设部门提供 +400%，完整数值见下表。
- 产业链按公司职责分工，避免同一建筑重复叠加“建筑群 + 单体建筑”吞吐量，并保留价格、供需和扩建决策。
- 所有公司均为玩家专属，脚本使用 `ai_will_do = { always = no }`；年度项目也只面向玩家。

将模组加入播放集并启用。成立公司需要研究对应技术，并在任一州拥有至少一个达到 5 级的合格建筑；具体名单见各公司条目，扩展建筑不自动计入成立条件。

2.0.0 已删除旧 MTM 公司且不提供兼容壳，2.0.0 之前的存档不保证兼容，建议开始新游戏。对于已经使用当前公司结构的存档，新增年度事件会在重新启动游戏并加载更新后的模组后，于下一次符合条件的年度检查初始化；真实存读档表现仍需游戏内验收。

### 首任执行官

本功能在启用后捕获公司成立事件，于次日自动任命对应的历史企业家，不要求繁荣，也不依赖历史人物 DLC 的自动候选池。八位企业家生成时统一为 **18 岁、知识分子利益集团、公司所属国家的主流文化与国教、随机 DNA 外貌**。年龄随后正常增长；文化与宗教不会随之后的国家变化自动重设。

| 公司 | 首任执行官 | 历史籍贯对应州 |
|---|---|---|
| 全球汽车工业集团 | 卡尔·本茨 | `STATE_BADEN` |
| 国际商业集团 | 亨利·约翰·亨氏 | `STATE_PENNSYLVANIA` |
| 环球建设集团 | 阿尔弗雷德·诺贝尔 | `STATE_SVEALAND` |
| 军工联合体 | 海勒姆·史蒂文斯·马克沁 | `STATE_MAINE` |
| 石油化工巨头 | 卡尔·博世 | `STATE_NORTH_RHINE` |
| 国际金融集团 | 内森·迈耶·罗斯柴尔德 | `STATE_HESSE` |
| 科技工业集团 | 杰拉德·飞利浦 | `STATE_GELRE` |
| 全球矿业资源集团 | 奥古斯特·蒂森 | `STATE_NORTH_RHINE` |

登场时固定添加六项特质：`charismatic`、`meticulous`、`innovative`、`persistent`、`expert_entrepreneur`、`imposing`。关闭初始随机特质生成，使用 `add_trait` 绕过正常成长的性格特质数量上限。后续原版疾病、事件、年龄和执行官任期规则继续生效；不强制将他们设为知识分子利益集团领袖。

每位创始人在本局只能通过这套成立流程任命一次。全局标记保存在存档中，解散重建或换国家成立均不重置。若公司在等待任命的一天内被解散或转移，任命取消，首次机会仍已消耗。更新前已有公司不补发、不替换现任；更新前已经解散的公司历史无法追溯，计数从启用本功能后首次捕获的成立事件开始。

任命在人物创建结束后、仍保留公司引用的事件上下文中执行，并核对公司现任执行官后才记录成功。若存档曾使用有问题的旧版回调，出现“人物已生成但未上任”，重启加载修复版后，下次月度国家检查会直接任命已生成的对应人物，不重复创建；正常离任的人物不会被重新任命。

实现位于 `super_company_founders` 人物模板、成立钩子、脚本效果、隐藏事件和中英姓名本地化文件中。追加成立及月度修复钩子，不覆盖原版人物、特质或全局规则。

<a id="zh-companies"></a>

### 公司效果详解

以下逐项列出当前公司脚本的 `prosperity_modifier`，即公司繁荣加成。脚本值 `1.25` 表示 +125%，`2.5` 表示 +250%；没有百分号的数值是直接增加值。年度开发事件只要求公司已成立且仍然存在，**不要求繁荣**，也没有额外的事件科技门槛。

本 README 的数值以当前脚本为准，已对齐早期独立说明与现有配置的差异：国际商业集团为五类粮食农场各 +500%、牧场 +200%、出生率 +100%；环球建设集团为建设部门 +400%、州建设力 +100%；矿业集团另含健康危害减免修饰项。此整合仅更新文档，不改动任何游戏数值。

#### 1. 全球汽车工业集团

- **解锁技术：** 内燃机。
- **成立建筑：** 汽车厂、动力机械厂、橡胶种植园。任一州至少有一个达到 5 级。

| 繁荣效果 | 数值 |
|---|---|
| 橡胶种植园吞吐量 | +250% |
| 发动机工业吞吐量 | +150% |
| 汽车工业吞吐量 | +125% |
| 陆军攻击力 | +10% |
| 州贸易优势 | +30% |

**声望商品：** 福特汽车（原版）、希肖发动机（原版）、米其林橡胶，共 3 种。汽车和发动机使用原版商品 key、名称和图标。搭配当前版本的 Cheat Menu Pro 时，汽车的名贵变体总数为 3 种，符合引擎上限。Cheat Menu Pro 另增一种发动机名贵变体，使发动机合计 4 种，仍有兼容性冲突。

产业链为橡胶 → 发动机 → 汽车，用更强的上游供给支撑汽车产能，并吸收旧 MTM 的发动机功能。

#### 2. 国际商业集团

- **解锁技术：** 公司章程。
- **成立建筑：** 纺织厂、食品厂、咖啡种植园、畜牧场、小麦农场、黑麦农场、玉米农场、水稻农场、渔业码头、棉花种植园、糖料作物种植园、鸦片种植园、茶叶种植园。任一州至少有一个达到 5 级。
- **其他经营建筑：** 捕鲸站、丝绸种植园、烟草种植园、葡萄园。这些建筑单独达到 5 级不满足成立条件。
- **扩展建筑：** 港口。

| 繁荣效果 | 数值 |
|---|---|
| 粮食农场吞吐量：黑麦、小麦、水稻、玉米、黍类 | +500% |
| 牧场吞吐量 | +200% |
| 种植园建筑群吞吐量 | +125% |
| 渔业吞吐量 | +125% |
| 食品工业吞吐量 | +100% |
| 纺织厂吞吐量 | +100% |
| 港口吞吐量 | +125% |
| 最大公司数 | +1 |
| 出生率 | +100% |
| 同化 | +90% |
| 影响力 | +30% |
| 移民吸引力 | +20% |
| 州贸易优势 | +20% |

**声望商品：** 名牌服装、京都和服、坎贝尔汤品、百加得朗姆酒（原版）、蓝山咖啡、优质谷物、精选鱼类、海岛棉、高纯鸦片、阿萨姆茶、雪茄、高速商船，共 12 种。

承担农牧渔业、食品、纺织等人口消费品供给，以及港口与商船物流；旧 MTM 的港口和商船功能并入本公司。

**永久教育计划：全民教育网络。** 玩家国家的国际商业集团达到游戏判定的繁荣状态后，在下一次每月国家检查永久解锁；自动为本国每个已整合州添加 **受教育机会 +50 个百分点、识字率增长 +0.10%** 的州修正，无须点击事件。同一州只添加一次，不逐月叠加。识字率增长脚本值为 `0.001`，是原版基础增长修正 `0.0005` 的两倍额外值；它不是立即提高识字率，也不保证每月直接增加 0.10 个百分点。

解锁标记与州修正随存档保存。解锁后，即使集团失去繁荣或解散，已授予的修正和后续补发资格仍保留；新整合或新获得的已整合州会通过整合、易主钩子及每月检查补发，未整合州不发放。分割州中的本国已整合部分同样符合条件，因为修正只作用于本国的州范围。已授予的修正没有期限，也不随州易主主动移除。已有存档更新后，若集团当前已繁荣，可在下一次每月检查解锁；更新前曾经繁荣但目前不繁荣的历史无法追溯。

**首次繁荣事件：众资成业。** 玩家国家的国际商业集团首次在每月国家检查中被检测为繁荣时，若“股份公司”、“银行业”或“中央银行”尚未研究，触发一次事件 `super_company_commercial.1`。确认或超时选择默认选项后，“股份公司”增加 **8000 研究进度**（第二时代基础费用 10000 的 **80%**），“银行业”增加 **6000 研究进度**（第一时代基础费用 7500 的 **80%**），“中央银行”增加 **8000 研究进度**（第二时代基础费用 10000 的 **80%**）。三项奖励分别检查并跳过已完成的科技；与已有进度相加，足够时可完成研究，而非将进度设置为 80%。研究费用修正和跨时代惩罚可能使界面显示的实际百分比不同。

事件弹出时即保存国家级一次性标记，繁荣下降、公司解散重建、存读档均不重发。三项科技均已研究时不弹事件；事件待处理期间若其中一项完成，则仅跳过该项奖励。已有存档当前已繁荣且事件尚未触发时可在下一次每月检查触发，更新前已经结束的繁荣无法追溯；已触发过事件的存档不重置标记、不补发新增奖励；现有全民教育网络独立照常生效。

#### 3. 环球建设集团

- **解锁技术：** 公司章程。
- **成立建筑：** 玻璃厂、炸药厂、炼钢厂、家具制造厂。任一州至少有一个达到 5 级。
- **扩展建筑：** 铁矿。

| 繁荣效果 | 数值 |
|---|---|
| 玻璃、炸药、钢铁、家具吞吐量 | +125% |
| 建设部门吞吐量 | +400% |
| 州建设力 | +100% |
| 每周最大建设进度 | +15 |

**声望商品：** 蒂芙尼玻璃、精炼钢、迈森瓷器、时尚家具、英格兰家具、诺贝尔炸药，共 6 种。

将建设投入品集中配平，连接钢铁、玻璃、炸药、家具与建设施工。当前建设部门和州建设力采用比旧说明更高的数值。

#### 4. 军工联合体

- **解锁技术：** 后膛炮。
- **成立建筑：** 武器厂、火炮铸造厂、弹药厂、造船厂。任一州至少有一个达到 5 级。
- **扩展建筑：** 炸药厂。

| 繁荣效果 | 数值 |
|---|---|
| 轻武器、火炮、弹药、造船厂吞吐量 | +125% |
| 陆军攻击力与防御力（各） | +100% |
| 军事商品成本 | -80% |
| 海军入侵效率 | +60% |
| 全国舰船建造效率 | +60% |
| 主力舰、巡洋舰、鱼雷艇建造进度上限（各） | +60% |
| 补给舰建造效率 | +60% |
| 武装部队人口吸引力 | +30% |
| 军事科技扩散 | +60% |

**声望商品：** 强力轻武器（原版通用名贵轻武器）、克虏伯炮、温彻斯特弹药、巴尔的摩快帆船、克莱德造班轮，共 5 种。轻武器使用原版商品 key、名称和图标，使轻武器的名贵变体总数保持在引擎的 3 种上限内。

供应轻武器、火炮、弹药与舰船，并接收旧 MTM 的造船、海军和军事科技功能。1.13 已合并民船与军舰造船厂，脚本统一使用 `building_shipyard`。

#### 5. 石油化工巨头

- **解锁技术：** 泵水机。
- **成立建筑：** 油井、化学合成厂、肥料厂、铁路。任一州至少有一个达到 5 级。
- **扩展建筑：** 染料作物种植园。

| 繁荣效果 | 数值 |
|---|---|
| 石油开采吞吐量 | +250% |
| 化工厂与合成材料厂吞吐量（各） | +150% |
| 铁路吞吐量 | +25% |
| 基础设施行业建设效率 | +100% |
| 基础设施行业雇员需求 | -75% |
| 州基础设施 | +50% |

**声望商品：** 标准石油、浓缩肥料、英国染料、高速铁路，共 4 种。英国染料使用自有商品 `prestige_good_british_dyes` 和 `british_dyes.dds` 图标，绑定基础商品 `dye`。原版德意志苯胺保留，当前模组组合下染料名贵变体共 2 种。

英国染料以英国合成染料工业为背景：British Dyes Limited 成立于 1915 年，1919 年与 Levinstein Limited 合并组建 British Dyestuffs Corporation；[科学博物馆保存的约 1920 年染料样品柜](https://collection.sciencemuseumgroup.org.uk/objects/co9224)提供了实物依据。模组沿用公司的名贵商品机制，解锁时间由游戏中的公司条件决定。

连接石油 → 化工与合成材料，并支持铁路运输，缓解后期石油、化肥、炸药和染料短缺。铁路仍需依靠等级提供基础设施；建设效率和低雇员需求支持更快、更便宜的大规模路网，降低工资成本，避免单级运输力爆量导致铁路亏损。

#### 6. 国际金融集团

- **解锁技术：** 中央银行。
- **成立建筑：** 造纸厂、工艺装备工坊、金融区。任一州至少有一个达到 5 级。
- **扩展建筑：** 伐木营地。

| 繁荣效果 | 数值 |
|---|---|
| 伐木业吞吐量 | +200% |
| 造纸厂与工具作坊吞吐量（各） | +150% |
| 最大公司数 | +1 |
| 税收收入 | +50,000 |
| 贷款利率 | -10% |
| 私人建设分配 | +10% |
| 铸币 | +10% |
| 农民、店主、教士、贵族、资本家投资池贡献（各） | +10% |
| 国有化投资回报、政府股息再投资、政府股息效率（各） | +10% |
| 实业家支持度 | +3 |

**声望商品：** 工艺纸品、瑞士工具、黄檀木、西伯利亚松木，共 4 种。

承担木材 → 纸张与工具链，以及金融和投资功能，并接收旧 MTM 的实业家支持度。旧版伐木业 +800% 已降为当前 +200%。

#### 7. 科技工业集团

- **解锁技术：** 发电。
- **成立建筑：** 发电厂、电子厂。任一州至少有一个达到 5 级。
- **扩展建筑：** 艺术学院、大学。

| 繁荣效果 | 数值 |
|---|---|
| 发电厂吞吐量 | +150% |
| 电气工业吞吐量 | +125% |
| 艺术学院与大学吞吐量（各） | +100% |
| 艺术学院雇员需求 | -75% |
| 城市中心吞吐量 | +20% |
| 最大公司数 | +1 |
| 创新上限 | +20 |
| 每周创新 | +60% |
| 官僚机构产出 | +20% |
| 影响力 | +30% |

**声望商品：** 爱迪生电力、马可尼无线电、大战略游戏，共 3 种。

连接电力 → 电气，并支持大学、创新与行政能力；吸收旧 MTM 的每周创新和额外公司槽。发动机归汽车集团负责，避免职责重叠。

#### 8. 全球矿业资源集团

- **解锁技术：** 炼钢。
- **成立建筑：** 铁矿、煤矿、硫矿、铅矿。任一州至少有一个达到 5 级。
- **扩展建筑：** 金矿。

| 繁荣效果 | 数值 |
|---|---|
| 矿业建筑群吞吐量，含核心矿业及金矿 | +200% |
| 污染对人群健康的危害减免 | -80% |

**声望商品：** 厄勒格伦德精铁、西里西亚煤炭、力拓硫矿、布罗肯希尔铅矿，共 4 种。

作为铁、煤、铅、硫与黄金等全产业链的资源底座，统一使用矿业建筑群加成，不再叠加“矿业群 + 单矿 + 金矿”多层吞吐量。健康危害减免一项按当前脚本原值 `state_pollution_reduction_health_mult = -0.8` 列出。


<a id="zh-chains"></a>

### 产业链分工

| 上游供给 | 中游加工 | 下游需求与支持 |
|---|---|---|
| 矿业建筑群 +200% | 钢铁、玻璃、炸药、家具各 +125% | 建设部门 +400%，州建设力 +100% |
| 橡胶 +250% | 发动机 +150% | 汽车 +125% |
| 石油 +250% | 化工、合成材料各 +150% | 铁路 +25%，基础设施行业建设效率 +100%、雇员需求 -75% |
| 粮食农场 +500%，牧场 +200%，种植园与渔业各 +125% | 食品、纺织各 +100% | 人口消费与贸易，港口 +125% |
| 木材 +200% | 纸张、工具各 +150% | 行政、建设与工业维护 |
| 建设与石化链提供军工投入品 | 武器、火炮、弹药、造船各 +125% | 陆海军战略加成 |

多数产业链保持上游供给强于加工与消费端；粮食和建设使用当前脚本中的更高数值。铁路依靠低成本扩建形成规模，矿业使用统一建筑群加成，避免旧版多重叠层。

<a id="zh-events"></a>

### 五个年度开发事件

| 已成立且仍存在的公司 | 事件 | 每年选择一个州后的永久效果 |
|---|---|---|
| 国际商业集团 | 大地的织机 | +100 可耕土地 |
| 国际金融集团 | 会生长的抵押品 | +50 级伐木营地资源上限 |
| 全球矿业资源集团 | 地底的第二次敲击 | 铁、煤、铅、硫、黄金五选一，+50 级对应矿场资源上限 |
| 全球汽车工业集团 | 把雨季装进玻璃 | +50 级橡胶种植园资源上限 |
| 石油化工巨头 | 第零号油田 | +50 级油井资源上限 |

#### 国际商业集团：大地的织机

集团在粮商订货会上展示“地表织机”：银色的编土结节是微型土壤装配器，能将碎岩、河泥和有机余料编成蓄水、通气、可供根系生长的耕作层，再接入当地灌溉网络，把原本无法耕种的荒地变为农田。故事采用订货会、收货单和地表工程队的视角，体现集团将新田野纳入商品供应网络的动机。

> “请在收货单上填写目的地。”<br>
> “货物是什么？”<br>
> “下一季的田野。”

流程：年度提案选择“批准部署，选定新的田野” → 分页选州 → 审阅部署委托 → “启动地表织机，登记新耕地”。最终确认后，指定州永久增加 **100 可耕土地**。可以选择“封存装置，暂缓本年度部署”；新增耕地上的农场仍需正常建设与雇工。

#### 国际金融集团：会生长的抵押品

集团发行“年轮债券”，把资本投入脉冲电场与共生菌育林实验。实验苗圃中的速生树种与分区轮伐，使银行家的新抵押品成为不断生长的森林。文案使用股东大会、资产评估和投资契约的口吻，带少量生物技术幻想。

> “现金躺在金库里不会长大。我们的抵押品每天都在长高。”

流程：年度提案 → 分页选州 → 审阅育林契约 → 签署。所选州永久增加 **50 级伐木营地资源上限**。现有营地保持原样，不直接发放木材，不改变农田或其他资源。

#### 全球矿业资源集团：地底的第二次敲击

一封深夜密电报告了停钻后仍在继续的敲击声。“回声引擎”由重力摆、晶体振荡器与差分机构成，将地层微震译为三维剖面，为过去无法利用的深部矿层寻找可开采通道。叙述采用密电、测量记录和现场指令，气氛偏向地下探索与未知回声。

> “我们发出的是两次脉冲。第三次是谁敲的？”

流程：年度提案 → 分页选州 → 从**铁、煤、铅、硫、黄金**中选择一种 → 审阅并启动。每年只为所选州的一种矿产永久增加 **50 级对应矿场资源上限**，其他矿种不变。确认前可改选矿种或返回重选州。黄金对应可建金矿，**不生成淘金地**，也不依赖随机资源发现。

#### 全球汽车工业集团：把雨季装进玻璃

汽车试验场不断改良发动机，轮胎供应却仍受远方雨季牵制。“恒夏计划”把光谱调节膜、保温穹顶和闭路水循环组成可运输的微型热带环境，让胶林跟随汽车供应链扩展。故事从试车场的供应难题展开，偏向材料工程与气候设施。

> “试车员问新轮胎能跑多远。工程师把一扇结霜的窗户推开：先看看它是在哪里长出来的。”

流程：年度提案 → 分页选州 → 审阅供货开发委托 → 确认。所选州永久增加 **50 级橡胶种植园资源上限**，不占用可耕土地，不改变其他资源。

#### 石油化工巨头：第零号油田

设计室递交一份没有标注油藏的井场蓝图。“地层蒸馏网”通过定向热井、循环催化液与集油通道，把低品位含碳层转化为可开发油源。叙述采用工程设计、工艺交付和控制室记录，强调以新的工艺建立油源，与其他项目的技术设定分别展开。

> “这张图上怎么没有油田？”<br>
> “请看下一张。那是工程结束后的地图。”

流程：年度提案 → 分页选州 → 审阅部署指令 → 确认。所选州永久增加 **50 级油井资源上限**，其他资源保持原值。

#### 所有年度项目的共同规则

1. 对应公司必须实际成立并仍然存在，单纯解锁不满足条件。只面向玩家，无须繁荣。
2. 选择单位为州，只允许本国**完整拥有且已完成整合**的州；未整合州、外国州和分割州不能选择。土地与资源上限作用于整个州地区，因此排除分割州，以保证增加量属于本国。
3. 资源州不必原本拥有对应资源；也可从零建立新的 50 级资源潜力。新增建筑仍受原版解锁科技约束，包括橡胶加工和抽油机。
4. 首次提案在成立公司后的下一次符合条件的年度国家检查触发，**不固定为 1 月 1 日**。各项目每年独立提供一次机会，五家公司同时存在时，五个项目可分别使用。
5. 每页最多四个州，支持前后翻页和返回重选；矿业还可重选矿种。浏览与预览不消耗机会，最终确认前不改变土地或资源。
6. 名单基于本次计划建立的候选州。打开选择页时重新过滤原始名单，失去或不再符合条件的州被移除；名单缩短导致当前页失效时返回第一页。
7. 确认前再次检查公司、州归属、完整持有、整合状态和有效矿种。条件失效时不能执行过期委托，可返回重选或暂缓。
8. 所有阶段均可暂缓，**超时也默认暂缓**，不会自动开发。完成或暂缓后当年不再发放；已有未处理提案时不重复发放。
9. 同一州可在不同年度重复开发，效果永久累加，并保存进存档。
10. 对应公司承担开发或勘探费用，不扣国库资金。增加的是土地或资源上限，**不会自动建成农场、伐木营地、矿场、橡胶种植园或油井，也不直接发放商品**；后续建设、雇工仍按正常规则进行。
11. 各公司项目和各国的名单、年度、进度分别保存，不互相占用机会，不使用全局变量或写入州变量。
12. 现有存档添加年度事件后，需重新启动游戏、加载更新后的模组，再等待年度检查。已增加的土地和资源容量随存档保留；早于 2.0.0 的公司存档兼容性仍按上文说明处理。

<a id="zh-compatibility"></a>

### 兼容性与实现

- 不再覆盖原版 `goods` 文件，因此与修改商品、运输力、电力和生活需求的模组更容易共存。公司类型与自有声望商品仍可能和使用相同 key 的模组发生覆盖冲突。
- 1.13 已弃用的 `manowars` / `ironclads` 声望商品绑定、旧军用造船厂与葡萄种植园 key，以及 12 个失效 modifier 已移除。钢铁、家具、造船修饰符采用 1.13 的单数 building key。
- MTM 公司已完全删除，其仍有意义的发动机、港口商船、造船海军、军事科技、创新、公司槽和实业家效果已按职责并入八家公司，不提供旧存档兼容壳。
- 年度项目只在独立文件中向 `on_yearly_pulse_country` 的 `on_actions` 列表追加各自钩子，不重新定义原版钩子的 `trigger` 或 `effect`。
- 永久教育计划使用独立的 `super_company_education` 修正、触发器、效果与本地化文件，向每月国家检查、州整合和州易主钩子追加动作；不依赖临时的 `prosperity_modifier`，不覆盖原版钩子的效果。
- 首次繁荣研究事件使用独立的 `super_company_commercial` 文件，向每月国家检查追加动作，通过国家变量防止重复；使用原版 `add_technology_progress` 分别增加 8000 点股份公司、6000 点银行业与 8000 点中央银行研究进度。
- 年度事件未覆盖原版地图、州历史、建筑或界面，没有使用 `replace_path`，也未改动公司定义来实现这些项目。
- 可耕土地使用 `state_region = { add_arable_land = 100 }`；资源项目使用 `change_resource_potential = { type = 对应建筑 amount = 50 }`，直接改变资源上限，不依赖吞吐量修正模拟资源。
- 橡胶与油井通过 `bg_extraction` 继承 `capped_by_resources`。不调用 `force_resource_discovery`，避免顺带发现原有地下储量，混淆指定的 50 级增量。真实游戏中仍需核对资源上限和发现机制的交互。
- 年度事件均提供简体中文与英文本地化；本 README 也完整提供这两种语言。

| 项目文件前缀 | 资源建筑 key | 事件流程编号 |
|---|---|---|
| `super_company_reclamation` | 可耕土地，不对应新建筑 | `.1` 提案 → `.2` 选州 → `.3` 确认 |
| `super_company_forestry` | `building_logging_camp` | `.1` 提案 → `.2` 选州 → `.3` 签署 |
| `super_company_minerals` | `building_iron_mine` / `building_coal_mine` / `building_lead_mine` / `building_sulfur_mine` / `building_gold_mine` | `.1` 提案 → `.2` 选州 → `.4` 选矿 → `.3` 启动 |
| `super_company_rubber` | `building_rubber_plantation` | `.1` 提案 → `.2` 选州 → `.3` 确认 |
| `super_company_oil` | `building_oil_rig` | `.1` 提案 → `.2` 选州 → `.3` 确认 |

完整事件编号由前缀和后缀组成，例如 `super_company_reclamation.1`。后续事件依赖前序提案创建的名单和上下文，不应单独触发。各项目文件分别位于 `common/on_actions`、`common/scripted_triggers`、`common/scripted_effects`、`events` 和 `localization`。

<a id="zh-validation"></a>

### 游戏内验收与验证范围

当前已做脚本结构、本地化与流程模拟检查；它们不能代替游戏引擎验收。更新模组后重新启动游戏，并核对：

1. 未成立对应公司时不触发；成立后在年度检查获得提案；五个项目同时存在时进度互不影响。
2. 名单只包含本国完整拥有的已整合州。候选州超过四个时，检查前后翻页、返回重选、最后一页不足四个，以及没有漏项或重复。
3. 垦殖确认前后，所选州可耕土地准确增加 100，其他州不变。
4. 林业准确增加 50 级伐木资源。五种矿产分别测试，每次只增加所选矿种 50 级；黄金应增加可建金矿上限，不出现淘金地或等待发现。
5. 橡胶与油井分别在无资源、已有资源、仍有未发现储量的州测试，确认对应可开发上限准确增加 50，未改变其他州、资源或原有地下储量。
6. 确认没有自动建成建筑或发放商品，原有已建建筑保持原值；资源项目不改变原有可耕土地。
7. 暂缓、超时、完成后当年不再发放，下一年可以再次收到；同一州跨年开发会累加。
8. 确认前解散公司、失去目标州、州被分割或失去整合资格时，无法执行过期委托。
9. 在选州、矿业选矿和最终确认阶段保存并读档，核对上下文恢复、真实弹窗布局和年度触发时机。

10. 新成立八家公司，经过一天后分别核对姓名、18 岁、知识分子、国家主流文化与国教、历史籍贯和六项特质；同日成立多家公司时不能串线。
11. 在成立当日存读档后推进一天，检查延迟任命；解散重建和执行官继任不得再次生成创始人。等待期间解散公司，不应生成无所属公司的创始人；更新前已有公司保留现任。
12. 国际商业集团未繁荣时不发放教育修正；繁荣后的下一次每月国家检查，为本国所有已整合州发放且每州仅一份，未整合州与外国州不发放。核对受教育机会 +50 个百分点、识字率增长 +0.10% 和永久期限；之后繁荣下降、解散公司、再次繁荣与存读档均不撤销或叠加，新整合州仍自动补发。

13. 国际商业集团未繁荣或股份公司、银行业、中央银行均已研究时不弹研究事件；首次符合条件的每月检查只弹一次。以无费用修正的国家核对三项科技各由 0% 增至 80%，已有至少 20% 时足以完成；任意一项已完成时，其余未完成项仍正常获得奖励。事件未处理时经过新的月度检查、再次繁荣、解散重建与存读档均不得重复发放。科技在事件处理前自然完成时仅跳过对应奖励。

真实游戏中的首任任命、头像、资源变更、发现机制、弹窗排版与存读档表现仍待验收。版本历史参见 [CHANGELOG.md](CHANGELOG.md)。

[返回语言选择](#top) · [English](#english)

---

<a id="english"></a>

## English

[Overview and installation](#en-overview) · [Company effects](#en-companies) · [Industrial chains](#en-chains) · [Annual events](#en-events) · [Compatibility and implementation](#en-compatibility) · [In-game validation](#en-validation)

<a id="en-overview"></a>

### Overview and installation

- Mod version: 2.0.0. Target game version: Victoria 3 1.13.11; metadata supports `1.13.*`.
- Eight companies cover resources, basic industry, construction, consumer goods, armaments, energy, research, and finance, with intentionally overpowered bonuses.
- Most core industries receive +100% to +250% throughput. The current script also grants +500% to grain farms and +400% to construction sectors; all values are listed below.
- Companies have distinct supply-chain roles. The design avoids duplicated building-group and individual-building throughput stacking while retaining prices, supply constraints, and expansion decisions.
- All companies are player-only through `ai_will_do = { always = no }`. Annual development projects also require a player country.

Add the mod to an enabled playset. Establishing a company requires its technology and at least one qualifying building at level 5 in any one state. See each company’s qualifying list; extension buildings do not automatically count toward establishment.

Version 2.0.0 removes the old MTM company without a compatibility shell. Saves from before 2.0.0 are not guaranteed to work, so a new campaign is recommended. For saves already using the current company roster, added annual events initialize at the next eligible annual check after restarting the game and loading the updated mod. Real save/load behavior still needs in-game validation.

### Founding executives

After this feature is enabled, a captured company-establishment event schedules the corresponding historical entrepreneur's appointment for the following day. Prosperity and historical-character DLC candidate pools are not required. All eight founders are created at **age 18, in the Intelligentsia, with the appointing country's primary culture and state religion, and random DNA**. They age normally; later country changes do not automatically reset their culture or religion.

| Company | Founding executive | Historical home region |
|---|---|---|
| Global Automotive Industries | Carl Benz | `STATE_BADEN` |
| International Commercial Group | Henry John Heinz | `STATE_PENNSYLVANIA` |
| Global Construction Group | Alfred Nobel | `STATE_SVEALAND` |
| Military Industrial Complex | Hiram Stevens Maxim | `STATE_MAINE` |
| Petrochemical Giant | Carl Bosch | `STATE_NORTH_RHINE` |
| International Financial Group | Nathan Mayer Rothschild | `STATE_HESSE` |
| Tech Industrial Group | Gerard Philips | `STATE_GELRE` |
| Global Mining Resources Group | August Thyssen | `STATE_NORTH_RHINE` |

Each starts with exactly six scripted traits: `charismatic`, `meticulous`, `innovative`, `persistent`, `expert_entrepreneur`, and `imposing`. Initial random trait generation is disabled; explicit `add_trait` effects bypass normal personality-trait growth limits. Vanilla illness, event, aging, and executive-career rules still apply afterward. Founders are not forcibly made Intelligentsia leaders.

Each founder can be appointed through this establishment flow only once per campaign. Global flags persist in the save, including after dissolution or establishment in another country. Dissolving or transferring the company during the one-day delay cancels the appointment and still consumes the first opportunity. Companies already present when updating receive no replacement executive. Earlier dissolved-company history cannot be reconstructed; tracking starts with the first establishment captured after enabling the feature.

Appointment runs after character creation in the event context that retains the company reference. Success is recorded only after checking the company's actual executive. Saves affected by the old callback bug reuse their already-created, unassigned founder at the next monthly country pulse after restarting with the fix; no duplicate is created and confirmed founders are not reinstated after normal succession.

Implementation uses `super_company_founders` character templates, appended establishment and monthly recovery actions, scripted effects, hidden events, and English/Simplified Chinese name localization. Vanilla characters, traits, and global defines are not overridden.

<a id="en-companies"></a>

### Company effects

The tables cover every entry in the current company scripts’ `prosperity_modifier` blocks: these are **company prosperity bonuses**. Script values `1.25` and `2.5` mean +125% and +250%; entries without a percent sign are flat additions. Annual development events require an established, existing company, **not prosperity**, and add no event-specific technology requirement.

This README follows the current scripts where they differ from the earlier standalone notes: the International Commercial Group grants +500% to each of five grain farm types, +200% to livestock ranches, and +100% birth rate; the Global Construction Group grants +400% construction sector throughput and +100% state construction efficiency. The mining company also has a health-effects-reduction modifier. This consolidation changes documentation only, not gameplay values.

#### 1. Global Automotive Industries

- **Technology:** Combustion Engine.
- **Qualifying buildings:** Automotive Industries, Motor Industries, Rubber Plantations. At least one must reach level 5 in any one state.

| Prosperity effect | Value |
|---|---|
| Rubber plantation throughput | +250% |
| Motor industry throughput | +150% |
| Automotive industry throughput | +125% |
| Army offense | +10% |
| State trade advantage | +30% |

**Prestige goods:** Ford Automobiles (vanilla), Schichau Engines (vanilla), Michelin Rubber (3 goods). Automobiles and engines use vanilla good keys, names, and icons. With the current version of Cheat Menu Pro, there are three prestige automobile variants, within the engine limit. Cheat Menu Pro also adds a prestige engine variant, bringing engines to four; that compatibility conflict remains.

The rubber → engines → automobiles chain uses stronger upstream supply to support automotive output. It also absorbs the former MTM company’s engine role.

#### 2. International Commercial Group

- **Technology:** Corporate Charters.
- **Qualifying buildings:** Textile Mills, Food Industries, Coffee Plantations, Livestock Ranches, Wheat Farms, Rye Farms, Maize Farms, Rice Farms, Fishing Wharves, Cotton Plantations, Sugar Plantations, Opium Plantations, Tea Plantations. At least one must reach level 5 in any one state.
- **Other company buildings:** Whaling Stations, Silk Plantations, Tobacco Plantations, Vineyards. These buildings alone do not satisfy the level-5 establishment condition.
- **Extension buildings:** Port.

| Prosperity effect | Value |
|---|---|
| Grain farm throughput: rye, wheat, rice, maize, and millet | +500% |
| Livestock ranch throughput | +200% |
| Plantation building-group throughput | +125% |
| Fishing wharf throughput | +125% |
| Food industry throughput | +100% |
| Textile mill throughput | +100% |
| Port throughput | +125% |
| Maximum companies | +1 |
| Birth rate | +100% |
| Assimilation | +90% |
| Influence | +30% |
| Migration attraction | +20% |
| State trade advantage | +20% |

**Prestige goods:** Designer Clothes, Kyoto Kimonos, Campbell Soup, Bacardi Rum (vanilla), Blue Mountain Coffee, Fine Grain, Select Fish, Sea Island Cotton, Pure Opium, Assam Tea, Cigars, Swift Merchant Marine (12 goods).

Supplies agricultural, livestock, fishing, food, and textile goods for population consumption, together with port and merchant-marine logistics. It absorbs MTM’s port and merchant shipping functions.

**Permanent education program: Universal Education Network.** Once the player's International Commercial Group satisfies the game's prosperous-company condition, the next monthly country check permanently unlocks the program. Every owned incorporated state automatically receives **+50 percentage points of education access and +0.10% literacy growth**, without an event choice. Each state receives one copy; monthly checks never stack it. Literacy growth uses `0.001`, an extra amount equal to twice the vanilla base modifier of `0.0005`; it does not immediately raise literacy or guarantee a flat 0.10-percentage-point monthly increase.

The country unlock and state modifiers are saved. Loss of prosperity or company dissolution preserves existing awards and eligibility for future awards. Incorporation and ownership-change hooks, backed by monthly checks, award newly incorporated or acquired incorporated states; unincorporated states receive nothing. An owned incorporated portion of a split region qualifies because this modifier operates on the owned state scope. Existing awards have no expiry and are not explicitly removed on ownership changes. In updated saves, a currently prosperous group can unlock the program at the next monthly check; prosperity that ended before this feature was installed cannot be reconstructed.

**First-prosperity event: Capital in Common.** The first monthly country check that detects the player's International Commercial Group as prosperous triggers `super_company_commercial.1`, provided at least one of Joint-Stock Companies, Banking, or Central Banking is not already researched. Choosing the default option, including on timeout, adds **8000 research progress** to Joint-Stock Companies (**80%** of its era-2 base cost of 10000), **6000** to Banking (**80%** of its era-1 base cost of 7500), and **8000** to Central Banking (**80%** of its era-2 base cost of 10000). Each award independently skips an already researched technology. Awards add to existing progress and can complete research; they do not set progress to 80%. Research-cost modifiers and era penalties can change the percentage shown in the interface.

A country marker is saved when the event opens, preventing repeat events after prosperity loss, dissolution, re-establishment, or save/load. No event appears if all three technologies are researched; research completing while the popup is pending skips only that technology's award. Currently prosperous existing saves with an unused milestone qualify at their next monthly check; prosperity that ended before this update cannot be reconstructed. Previously triggered events retain their marker and receive no retroactive additional award. The Universal Education Network continues independently.

#### 3. Global Construction Group

- **Technology:** Corporate Charters.
- **Qualifying buildings:** Glassworks, Explosives Factory, Steel Mills, Furniture Manufacturies. At least one must reach level 5 in any one state.
- **Extension buildings:** Iron Mines.

| Prosperity effect | Value |
|---|---|
| Glassworks, explosives, steel, and furniture throughput | +125% |
| Construction sector throughput | +400% |
| State construction efficiency | +100% |
| Maximum weekly construction progress | +15 |

**Prestige goods:** Tiffany Glass, Refined Steel, Meissen Porcelain, Stylish Furniture, English Upholstery, Nobel Explosives (6 goods).

Coordinates construction inputs across steel, glass, explosives, furniture, and construction sectors. Construction sector and state construction bonuses are higher in the current script than in the earlier standalone notes.

#### 4. Military Industrial Complex

- **Technology:** Breech-Loading Artillery.
- **Qualifying buildings:** Arms Industries, Artillery Foundry, Munition Plants, Shipyards. At least one must reach level 5 in any one state.
- **Extension buildings:** Explosives Factory.

| Prosperity effect | Value |
|---|---|
| Arms, artillery, ammunition, and shipyard throughput | +125% |
| Army offense and defense, each | +100% |
| Military goods cost | -80% |
| Naval invasion efficiency | +60% |
| National ship construction efficiency | +60% |
| Construction progress cap for capital ships, cruisers, and torpedo craft, each | +60% |
| Supply ship construction efficiency | +60% |
| Armed Forces pop attraction | +30% |
| Military technology spread | +60% |

**Prestige goods:** High-powered Small Arms (vanilla generic prestige small arms), Krupp Guns, Winchester Ammunition, Baltimore Clippers, Clyde-Built Liners (5 goods). Small arms use the vanilla good key, name, and icon, keeping the number of prestige small-arms variants within the engine's limit of three.

Supplies small arms, artillery, ammunition, and ships, and absorbs MTM’s shipbuilding, naval, and military technology roles. Version 1.13 combines civilian and military shipyards, so the script uses `building_shipyard`.

#### 5. Petrochemical Giant

- **Technology:** Pumpjacks.
- **Qualifying buildings:** Oil Rigs, Synthetics Plants, Fertilizer Plants, Railway. At least one must reach level 5 in any one state.
- **Extension buildings:** Dye Plantations.

| Prosperity effect | Value |
|---|---|
| Oil extraction throughput | +250% |
| Chemical and synthetics plant throughput, each | +150% |
| Railway throughput | +25% |
| Infrastructure building-group construction efficiency | +100% |
| Infrastructure building-group employee requirements | -75% |
| State infrastructure | +50% |

**Prestige goods:** Standard Oil, Enriched Fertilizer, British Dyes, High-Speed Rail (4 goods). British Dyes uses the custom good `prestige_good_british_dyes`, the `british_dyes.dds` icon, and the base good `dye`. Vanilla German Aniline is preserved; the current mod combination has two prestige dye variants.

British Dyes draws on Britain's synthetic dye industry: British Dyes Limited was founded in 1915 and merged with Levinstein Limited in 1919 to form British Dyestuffs Corporation. The [Science Museum's dye sample cabinet from circa 1920](https://collection.sciencemuseumgroup.org.uk/objects/co9224) provides surviving examples. In the mod, availability follows the company's normal prestige-good conditions.

Connects oil to chemicals and synthetic materials while supporting rail transport, addressing late-game oil, fertilizer, explosives, and dye shortages. Railways still need levels to supply infrastructure. Faster construction and lower staffing support a larger, cheaper network with lower wage costs, avoiding the losses caused by excessive transport output per railway level.

#### 6. International Financial Group

- **Technology:** Central Banking.
- **Qualifying buildings:** Paper Mills, Tooling Workshops, Financial Districts. At least one must reach level 5 in any one state.
- **Extension buildings:** Logging Camps.

| Prosperity effect | Value |
|---|---|
| Logging building-group throughput | +200% |
| Paper mill and tooling workshop throughput, each | +150% |
| Maximum companies | +1 |
| Tax income | +50,000 |
| Loan interest rate | -10% |
| Private construction allocation | +10% |
| Minting | +10% |
| Investment pool contributions from Farmers, Shopkeepers, Clergymen, Aristocrats, and Capitalists, each | +10% |
| Nationalization investment return, government dividend reinvestment, and government dividend efficiency, each | +10% |
| Industrialists approval | +3 |

**Prestige goods:** Craft Paper, Swiss Tools, Rosewood, Siberian Pine (4 goods).

Handles the timber → paper and tools chain, finance, and investment, including MTM’s former Industrialist approval bonus. The old +800% logging bonus is now +200%.

#### 7. Tech Industrial Group

- **Technology:** Electrical Generation.
- **Qualifying buildings:** Power Plants, Electrics Industries. At least one must reach level 5 in any one state.
- **Extension buildings:** Arts Academy, University.

| Prosperity effect | Value |
|---|---|
| Power plant throughput | +150% |
| Electrics industry throughput | +125% |
| Arts academy and university throughput, each | +100% |
| Arts academy employee requirements | -75% |
| Urban center throughput | +20% |
| Maximum companies | +1 |
| Innovation cap | +20 |
| Weekly innovation | +60% |
| Bureaucracy output | +20% |
| Influence | +30% |

**Prestige goods:** Edison Electricity, Marconi Radio, Grand Strategy Games (3 goods).

Connects power to electrical industry and supports universities, innovation, and administration. It absorbs MTM’s weekly innovation and additional company slot, while engines belong to the automotive company.

#### 8. Global Mining Resources Group

- **Technology:** Steelworking.
- **Qualifying buildings:** Iron Mines, Coal Mines, Sulfur Mines, Lead Mines. At least one must reach level 5 in any one state.
- **Extension buildings:** Gold Mines.

| Prosperity effect | Value |
|---|---|
| Mining building-group throughput, including the core mines and gold mines | +200% |
| Pollution Health Effects Reduction | -80% |

**Prestige goods:** Oregrounds Iron, Silesian Coal, Rio Tinto Sulfur, Broken Hill Lead (4 goods).

Provides the iron, coal, lead, sulfur, and gold resource base for the other chains. A single mining building-group modifier replaces stacked group, individual-mine, and gold-mine throughput bonuses. The health-effects-reduction entry is recorded exactly as configured: `state_pollution_reduction_health_mult = -0.8`.


<a id="en-chains"></a>

### Industrial-chain roles

| Upstream supply | Processing | Downstream demand and support |
|---|---|---|
| Mining building group +200% | Steel, glass, explosives, and furniture +125% each | Construction sectors +400%; state construction efficiency +100% |
| Rubber +250% | Engines +150% | Automobiles +125% |
| Oil +250% | Chemicals and synthetics +150% each | Railways +25%; infrastructure construction efficiency +100%, staffing -75% |
| Grain farms +500%, livestock +200%, plantations and fishing +125% each | Food and textiles +100% each | Population consumption and trade; ports +125% |
| Timber +200% | Paper and tools +150% each | Administration, construction, and industrial maintenance |
| Construction and petrochemical chains supply military inputs | Arms, artillery, ammunition, and shipbuilding +125% each | Army and navy strategic bonuses |

Most chains keep upstream supply stronger than processing and consumption, while grain and construction use the higher values in the current script. Railways scale through cheaper expansion, and mining uses one building-group bonus instead of the former stacked modifiers.

<a id="en-events"></a>

### Five annual development events

| Established company that must still exist | Event | Permanent effect in one chosen state each year |
|---|---|---|
| International Commercial Group | A Loom for the Land | +100 arable land |
| International Financial Group | Collateral That Grows | +50 levels of logging camp resource capacity |
| Global Mining Resources Group | The Second Knock Below | Choose iron, coal, lead, sulfur, or gold: +50 levels of the selected mine’s resource capacity |
| Global Automotive Industries | A Rainy Season Under Glass | +50 levels of rubber plantation resource capacity |
| Petrochemical Giant | Oilfield Number Zero | +50 levels of oil rig resource capacity |

#### International Commercial Group: A Loom for the Land

At a grain merchants’ trade fair, the Group unveils the Surface Loom. Silver ceramic nodules act as miniature soil assemblers, weaving crushed rock, river silt, and organic residue into a growing layer that retains water, admits air, and supports roots. Local irrigation turns previously unusable ground into farmland. Trade-fair demonstrations, delivery notes, and surface engineering crews frame the Group’s ambition to bring new fields into its supply network.

> “Please enter the destination on the delivery note.”<br>
> “What is the cargo?”<br>
> “Next season’s fields.”

Flow: annual proposal → “Authorize deployment and designate the new fields” → paginated state selection → contract review → “Activate the Surface Loom and register the new farmland.” Final approval permanently adds **100 arable land** to the selected state. Equipment can instead be stored to postpone the project. Farms on the new land still require normal construction and workers.

#### International Financial Group: Collateral That Grows

Tree-Ring Bonds finance pulsed electric fields and symbiotic fungi in experimental nurseries. Fast-growing trees and rotational woodland management turn the bankers’ collateral into a living forest. Shareholders’ meetings, asset valuations, and investment contracts give the story a financial voice with a touch of speculative biotechnology.

> “Cash does not grow in a vault. Our collateral grows taller every day.”

Flow: annual proposal → paginated state selection → review the forestry contract → sign. The selected state permanently gains **50 levels of logging camp resource capacity**. Existing camps are unchanged; the event does not grant timber goods or change farmland or other resources.

#### Global Mining Resources Group: The Second Knock Below

A confidential night telegram reports knocking that continues after the drill stops. The Echo Engine combines gravity pendulums, crystal oscillators, and a difference engine to translate tiny tremors into a three-dimensional section of the earth, finding extraction routes to previously inaccessible deposits. Telegrams, survey records, and site instructions give the story the atmosphere of underground exploration and unknown echoes.

> “We transmitted two pulses. Who made the third knock?”

Flow: annual proposal → paginated state selection → choose **iron, coal, lead, sulfur, or gold** → review and activate. Each year, one mineral in one state permanently gains **50 levels of the corresponding mine’s resource capacity**; all other minerals are unchanged. Before approval, change the mineral or return to choose another state. Gold means buildable gold mines: **no gold fields are created**, and the added capacity does not depend on a random discovery.

#### Global Automotive Industries: A Rainy Season Under Glass

Engines improve on the proving ground while tire supplies still depend on a distant rainy season. Project Perpetual Summer combines spectral control membranes, insulated domes, and closed-loop water circulation into portable tropical environments, allowing rubber groves to follow the automotive supply chain. The story approaches speculative materials and climate engineering through a test circuit’s supply problem.

> “The test driver asked how far the new tires could travel. An engineer opened a frost-covered window. First, see where they grew.”

Flow: annual proposal → paginated state selection → review the supply-development contract → approve. The selected state permanently gains **50 levels of rubber plantation resource capacity**, without using arable land or changing other resources.

#### Petrochemical Giant: Oilfield Number Zero

The design office submits a well-site blueprint without an oil reservoir. The Subsurface Distillation Grid uses directional thermal wells, circulating catalysts, and collection channels to turn low-grade carbon-bearing strata into a workable oil source. Design-office exchanges, process delivery, and control-room records distinguish its engineering story from the other projects.

> “Why is there no oilfield on this map?”<br>
> “See the next sheet. That is the map after the work is finished.”

Flow: annual proposal → paginated state selection → review the deployment instruction → approve. The selected state permanently gains **50 levels of oil rig resource capacity**. Other resources retain their previous values.

#### Shared rules for all annual projects

1. The corresponding company must actually be established and still exist. Unlocking it is insufficient. Projects are player-only and do not require prosperity.
2. Selection operates at state level. Only **fully incorporated states whose entire state region belongs to our country** are eligible. Foreign, unincorporated, and split states are excluded. Land and resource potential belong to the whole state region, so excluding split states keeps the full increase inside our country.
3. A state does not need to possess the resource already; projects can create 50 levels of potential from zero. Buildings still follow normal technology requirements, including Rubber Mastication and Pumpjacks.
4. The first proposal appears at the next eligible annual country check after establishment, **not necessarily January 1**. Each project offers its own opportunity each year; all five can be used independently when all five companies exist.
5. Each page shows at most four states, with forward/back navigation and reselection. Mining also allows mineral reselection. Browsing and previews do not consume the opportunity or change resources.
6. Candidates come from the list created for the current plan. Each selection page filters that original list again, removing lost or ineligible states. If the list shrinks beyond the current page, it returns to page one.
7. Final approval rechecks the company, ownership, complete possession, incorporation, and valid mineral choice. An expired contract cannot be executed; choose another eligible target or postpone.
8. Every stage allows postponement. **Timeout defaults to postponement**, never automatic development. Completion or cancellation prevents another offer that year; unresolved proposals also prevent duplicates.
9. The same state can be developed again in later years. Effects accumulate permanently and are stored in the save.
10. The company pays development or survey costs, with no treasury charge. These events add land or resource capacity, **not finished farms, logging camps, mines, rubber plantations, oil rigs, or goods**. Subsequent construction and staffing follow normal rules.
11. Each project and country keeps separate candidate lists, annual markers, and progress. Projects do not consume one another’s opportunities, use global variables, or write variables onto states.
12. After adding annual events to an existing save, restart the game, load the updated mod, and wait for the annual check. Added land and resource capacity persist in the save. Company compatibility for pre-2.0.0 saves remains as described above.

<a id="en-compatibility"></a>

### Compatibility and implementation

- The mod no longer overrides vanilla `goods` files, improving coexistence with mods that change goods, transportation, electricity, or needs. Company types and custom prestige goods can still conflict with mods using the same keys.
- Removed content includes deprecated `manowars` / `ironclads` prestige-good bindings, obsolete military-shipyard and vineyard keys, and 12 invalid modifiers. Steel, furniture, and shipyard modifiers use the singular building keys required by 1.13.
- MTM has been removed completely. Its useful engine, port and merchant-shipping, shipbuilding and naval, military-tech, innovation, company-slot, and Industrialist effects have been redistributed by company role. No legacy-save compatibility shell is provided.
- Independent annual-project files append their own actions to `on_yearly_pulse_country` through its `on_actions` list. They do not redefine the vanilla pulse’s `trigger` or `effect` blocks.
- The permanent education program has separate `super_company_education` modifiers, triggers, effects, and localization. It appends actions to monthly country, state-incorporation, and state-ownership-change hooks, independently of the temporary `prosperity_modifier`, without overriding vanilla hook effects.
- The first-prosperity research event uses separate `super_company_commercial` files, appends an action to the monthly country pulse, and uses a saved country variable to prevent repeats. Vanilla `add_technology_progress` independently adds 8000 points to Joint-Stock Companies, 6000 to Banking, and 8000 to Central Banking.
- Annual projects do not override vanilla maps, state history, buildings, or GUI files. They do not use `replace_path` or modify company definitions to implement the projects.
- Arable land uses `state_region = { add_arable_land = 100 }`. Resource projects use `change_resource_potential = { type = corresponding_building amount = 50 }` to change capacity directly, rather than approximating resources with throughput modifiers.
- Rubber and oil inherit `capped_by_resources` through `bg_extraction`. The projects do not call `force_resource_discovery`, avoiding incidental discovery of existing underground reserves that could obscure the intended 50-level increase. Real engine interaction between capacity and discovery still needs validation.
- Every annual event has Simplified Chinese and English localization. This README provides both languages in full.

| Project file prefix | Resource building key | Event flow |
|---|---|---|
| `super_company_reclamation` | Arable land, not a new building | `.1` proposal → `.2` state selection → `.3` confirmation |
| `super_company_forestry` | `building_logging_camp` | `.1` proposal → `.2` state selection → `.3` signature |
| `super_company_minerals` | `building_iron_mine` / `building_coal_mine` / `building_lead_mine` / `building_sulfur_mine` / `building_gold_mine` | `.1` proposal → `.2` state selection → `.4` mineral selection → `.3` activation |
| `super_company_rubber` | `building_rubber_plantation` | `.1` proposal → `.2` state selection → `.3` confirmation |
| `super_company_oil` | `building_oil_rig` | `.1` proposal → `.2` state selection → `.3` confirmation |

Combine a prefix and suffix to form a full event ID, such as `super_company_reclamation.1`. Follow-up events require candidate lists and context created by the proposal and should not be triggered individually. Project files live under `common/on_actions`, `common/scripted_triggers`, `common/scripted_effects`, `events`, and `localization`.

<a id="en-validation"></a>

### In-game validation and verification limits

Script structure, localization, and workflow simulations have been checked. They do not replace engine validation. Restart the game with the updated mod and verify:

1. No proposal without the corresponding company; annual proposals after establishment; independent progress when all five projects are available.
2. Only fully owned, incorporated states appear. With more than four candidates, test forward/back navigation, reselection, a partially filled final page, and no omitted or repeated states.
3. Reclamation adds exactly 100 arable land to the selected state, leaving other states unchanged.
4. Forestry adds exactly 50 levels of logging capacity. Test all five minerals separately: each adds 50 only to its selected mineral. Gold increases gold-mine capacity without gold fields or a discovery wait.
5. Test rubber and oil in states with no resource, existing resources, and undiscovered reserves. Confirm the target capacity increases by exactly 50, without changing other states, resources, or existing underground reserves.
6. No buildings or goods appear automatically; existing building levels remain unchanged. Resource projects leave existing arable land unchanged.
7. Postponement, timeout, and completion prevent another offer that year. Next-year offers work, and repeated development in later years accumulates.
8. Dissolving the company, losing the state, splitting it, or losing incorporation eligibility before approval prevents stale contracts from executing.
9. Save and reload during state selection, mineral selection, and final confirmation. Check context restoration, actual popup layout, and annual trigger timing.

10. Establish all eight companies and advance one day. Check names, age 18, Intelligentsia affiliation, primary culture, state religion, historical home regions, and six traits. Simultaneous foundations must retain separate targets.
11. Save on the establishment day, reload, and advance one day. Re-establishment and succession must not create another founder. Dissolution during the delay must not create an unattached founder; pre-existing companies retain their executives.
12. No education award before the International Commercial Group is prosperous. At the next monthly country check, all owned incorporated states receive exactly one permanent award, with +50 percentage points of education access and +0.10% literacy growth; unincorporated and foreign states do not. Check that prosperity loss, dissolution, renewed prosperity, and save/load neither remove nor stack awards, and later incorporations still receive them.

13. No research event before the International Commercial Group is prosperous or after all three of Joint-Stock Companies, Banking, and Central Banking are researched. The first eligible monthly check produces one event. Without cost modifiers, verify each technology rises from 0% to 80%, or completes from at least 20%. If any are complete, the remaining technologies must still receive their awards. Further monthly checks while pending, renewed prosperity, dissolution and re-establishment, and save/load must not duplicate it. Research completing before the option is resolved must skip only that technology's award.

Actual founder appointments, portraits, resource changes, discovery interaction, popup layout, and save/load behavior still require in-game acceptance testing. See [CHANGELOG.md](CHANGELOG.md) for version history.

[简体中文](#zh-cn)

---

### 数值来源 / Configuration reference

公司数值来源 / Company values: [00_stronger_basic_companies.txt](common/company_types/00_stronger_basic_companies.txt)。
