# EmberKnight — UE5 C++ ARPG 战斗 Demo

面向**游戏客户端开发岗**的简历项目：一个纯 C++ 驱动的第三人称 ARPG 战斗 Demo，
覆盖角色控制、连招战斗、AI、UI、关卡胜负的完整闭环。

**技术栈**：Unreal Engine 5.4 · C++ · Enhanced Input · 行为树/AIPerception · UMG · Git

## 核心特性

| 模块 | 实现 | 代码位置 |
|---|---|---|
| 第三人称角色 | Enhanced Input 映射上下文解耦输入；过肩相机 | `Characters/EKCharacter` |
| 三段连招 | 输入缓冲 + AnimNotify 连招窗口，逐段伤害倍率递增 | `Components/CombatComponent` |
| 伤害判定 | AnimNotifyState 帧判定窗口，球形扫掠，同目标单窗口去重 | `Animation/EKAnimNotifyState_AttackWindow` |
| 属性系统 | 纯 C++ ActorComponent（HP/受击/死亡），玩家敌人复用 | `Components/AttributeComponent` |
| 闪避 | 翻滚取消攻击，无敌帧（伤害结算前查询目标无敌状态） | `Components/CombatComponent` |
| 锁定相机 | 最近目标锁定，控制器朝向插值，死亡/超距自动脱锁 | `Components/LockOnComponent` |
| 敌人 AI | AIPerception 视觉感知写黑板，行为树编排巡逻/追击/攻击 | `AI/EKAIController` |
| UI | UMG 血条/伤害飘字，委托驱动数据绑定 | `UI/` |
| 关卡胜负 | TargetPoint 刷怪点刷怪，全灭胜利/死亡失败 | `Game/EKGameMode` |

## 架构设计要点（面试展开用）

- **组件化复用**：属性与战斗判定是 ActorComponent，玩家和敌人共用同一套逻辑，
  差异只在蒙太奇资产和数值参数。
- **动画驱动的判定**：伤害帧由动画资产上的 Notify 窗口决定而非代码定时器，
  调手感不用改代码；C++ 只提供执行能力。
- **输入缓冲连招**：攻击中预输入先缓存，连招窗口开启时自动衔接，避免"按了没反应"。
- **感知与决策分离**：AIPerception 只负责"看见谁"，追不追/怎么打在行为树里
  用装饰器控制，改 AI 策略不动 C++。

## 简历描述（可直接用）

> **EmberKnight — UE5 ARPG 战斗 Demo（个人项目）**
> - 基于 UE5.4 纯 C++ 实现第三人称 ARPG 战斗 Demo，使用 Enhanced Input 完成输入解耦，
>   角色移动/相机/攻击/闪避全 C++ 驱动
> - 设计组件化战斗系统：属性组件（HP/受击/死亡）玩家敌人复用；通过 AnimNotifyState
>   实现动画帧级伤害判定窗口，三段连招支持输入缓冲与逐段伤害倍率
> - 实现带无敌帧的翻滚闪避与基于最近目标搜索的锁定相机
> - 基于 AIPerception + 行为树实现敌人巡逻/追击/攻击状态切换，感知与决策分离
> - UMG 血条/伤害飘字采用委托驱动数据绑定，GameMode 管理刷怪点刷怪与胜负判定

## 运行方式

见 [Docs/01-环境搭建指南.md](Docs/01-环境搭建指南.md)（Windows + UE5.4 + VS2022）。
行为树资产搭建见 [Docs/03-行为树搭建指南.md](Docs/03-行为树搭建指南.md)。

## 仓库结构

```
EmberKnight.uproject
Source/EmberKnight/
├── Characters/   EKCharacter（玩家）/ EKEnemy（敌人）
├── Components/   AttributeComponent / CombatComponent / LockOnComponent
├── Animation/    EKAnimInstance / 战斗 Notify 们
├── AI/           EKAIController（AIPerception + 黑板）
├── UI/           HUD / 伤害飘字
├── Game/         EKGameMode
└── Core/         共享枚举（战斗状态机）
```

开发过程按天提交（tag: day1 ~ day13-14），commit 历史可展示。
