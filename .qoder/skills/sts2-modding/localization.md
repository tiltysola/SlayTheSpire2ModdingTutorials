# 本地化与描述系统

## 本地化文件位置

```
{modId}/
└── localization/
    ├── zh_CN/
    │   ├── cards.json
    │   ├── relics.json
    │   ├── powers.json
    │   ├── characters.json
    │   └── ancients.json
    └── en/
        └── ...
```

## BBCode 标签

### Godot 原生标签

| BBCode | 说明 | 示例 |
|--------|------|------|
| `[b]...[/b]` | 粗体 | `[b]重要[/b]` |
| `[i]...[/i]` | 斜体 | `[i]提示[/i]` |
| `[u]...[/u]` | 下划线 | `[u]强调[/u]` |
| `[color=...]...[/color]` | 颜色 | `[color=red]警告[/color]` |
| `[font=...]...[/font]` | 字体 | `[font=Arial]文本[/font]` |
| `[font_size=...]...[/font_size]` | 字号 | `[font_size=24]大字[/font_size]` |

### 游戏自定义标签

| 标签 | 效果 |
|------|------|
| `[ancient_banner]...[/ancient_banner]` | 古代横幅风格 |
| `[aqua]...[/aqua]` | 水绿色 |
| `[blue]...[/blue]` | 蓝色 |
| `[gold]...[/gold]` | 金色 |
| `[green]...[/green]` | 绿色 |
| `[orange]...[/orange]` | 橙色 |
| `[pink]...[/pink]` | 粉色 |
| `[purple]...[/purple]` | 紫色 |
| `[red]...[/red]` | 红色 |
| `[fade_in]...[/fade_in]` | 渐显动画 |
| `[fly_in]...[/fly_in]` | 飞入动画 |
| `[jitter]...[/jitter]` | 抖动动画 |
| `[sine]...[/sine]` | 正弦波动 |
| `[thinky_dots]...[/thinky_dots]` | 思考点点 |

## 占位变量

在描述中使用，会被 DynamicVar 的数值替换。

| 变量 | 对应类 | 示例 |
|------|--------|------|
| `{Damage}` | `DamageVar` | `造成{Damage:diff()}点伤害。` |
| `{Block}` | `BlockVar` | `获得{Block:diff()}点格挡。` |
| `{Cards}` | `CardsVar` | `抽{Cards:diff()}张牌。` |
| `{Energy}` | `EnergyVar` | `获得{Energy:energyIcons()}。` |
| `{Repeat}` | `RepeatVar` | `造成伤害{Repeat:diff()}次。` |
| `{Heal}` | `HealVar` | `回复{Heal:diff()}点生命。` |
| `{HpLoss}` | `HpLossVar` | `失去{HpLoss:inverseDiff()}点生命。` |
| `{MaxHp}` | `MaxHpVar` | `获得{MaxHp:diff()}点最大生命。` |
| `{Gold}` | `GoldVar` | `获得{Gold:diff()}金币。` |
| `{Stars}` | `StarsVar` | `获得{Stars:starIcons()}。` |
| `{StrengthPower}` | `PowerVar<StrengthPower>` | `获得{StrengthPower:diff()}点力量。` |
| `{WeakPower}` | `PowerVar<WeakPower>` | `给予{WeakPower:diff()}层虚弱。` |

## Formatter

格式化变量的显示方式。

| Formatter | 说明 | 示例 |
|-----------|------|------|
| `diff()` | 高于基础变绿，低于变红 | `{Damage:diff()}` |
| `inverseDiff()` | 高于基础变红，低于变绿 | `{HpLoss:inverseDiff()}` |
| `energyIcons()` | 渲染为能量图标 | `{Energy:energyIcons()}` |
| `starIcons()` | 渲染为辉星图标 | `{Stars:starIcons()}` |
| `abs` | 绝对值 | `{Damage:abs()}` |
| `percentMore()` | 百分比增加 | `{Boost:percentMore()}` |

### SmartFormat 内置 Formatter

| Formatter | 说明 | 示例 |
|-----------|------|------|
| `cond` | 条件分支 | `{X:cond:>0?生效\|不生效}` |
| `choose` | 按索引选择 | `{X:choose(1\|2):一\|二\|其他}` |
| `plural` | 复数（英语） | `{Cards:plural:card\|cards}` |
| `list` | 拼接列表 | 详见 SmartFormat 文档 |

## 卡牌上下文变量

| 变量 | 含义 | 示例 |
|------|------|------|
| `singleStarIcon` | 星星图标 | `每获得{singleStarIcon}时` |
| `InCombat` | 是否战斗中 | `{InCombat:\n（命中{CalculatedHits}次）\|}` |
| `IsTargeting` | 是否有目标 | `{IsTargeting:\n（造成{CalculatedDamage}）\|}` |
| `IfUpgraded` | 是否升级 | `{IfUpgraded:show:升级文本\|未升级文本}` |

## 先古对话格式

文件: `{modId}/localization/{Language}/ancients.json`

### ID 格式

```
{先古ID}.talk.{角色ID}.{对话序号}-{行号}[r].{ancient|char|next}
```

- `ancient`: 先古说的话
- `char`: 角色说的话
- `next`: 继续按钮文本
- `r`: 标记为可重复使用

### 示例

```json
{
  "DARV.talk.IRONCLAD.0-0.ancient": "啊，冒火的战士回来啦！",
  "DARV.talk.IRONCLAD.1-0r.ancient": "看到你还活着真不错！",
  "DARV.talk.IRONCLAD.2-0.ancient": "我还留着你的重刃呢……",
  "DARV.talk.IRONCLAD.2-0.next": "继续",
  "DARV.talk.IRONCLAD.2-1.char": "[i]铁甲战士盯着达弗。[/i]",
  "DARV.talk.firstVisitEver.0-0.ancient": "欢迎来到我的收藏！",
  "DARV.talk.ANY.0-0r.ancient": "来看看我的宝贝吧！",
  "DARV.title": "达弗",
  "DARV.epithet": "囤积者"
}
```

### 对话触发规则

- 第一次遇见：触发 `firstVisitEver` 对话
- 之后按角色遇见次数选择对话套数（0, 1, 2...）
- 带 `r` 标记的对话在没有匹配时可重复使用
- 角色不认识时触发 `ANY` 对话

## 人物本地化

文件: `{modId}/localization/{Language}/characters.json`

```json
{
  "MODID-CHARACTER_ID.title": "角色名",
  "MODID-CHARACTER_ID.titleObject": "角色名（宾语）",
  "MODID-CHARACTER_ID.description": "角色描述",
  "MODID-CHARACTER_ID.pronounSubject": "他",
  "MODID-CHARACTER_ID.pronounObject": "他",
  "MODID-CHARACTER_ID.pronounPossessive": "他的",
  "MODID-CHARACTER_ID.possessiveAdjective": "他的",
  "MODID-CHARACTER_ID.cardsModifierTitle": "卡池名",
  "MODID-CHARACTER_ID.cardsModifierDescription": "卡池描述"
}
```
