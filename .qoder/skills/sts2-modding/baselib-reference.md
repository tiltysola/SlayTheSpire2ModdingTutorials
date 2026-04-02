# BaseLib 详细参考

## 添加依赖

### .csproj 引用

```xml
<!-- 本地引用 -->
<Reference Include="BaseLib">
  <HintPath>$(Sts2Dir)/mods/BaseLib/BaseLib.dll</HintPath>
  <Private>false</Private>
</Reference>

<!-- 或 NuGet 引用 -->
<PackageReference Include="Alchyr.Sts2.BaseLib" Version="*" />
```

### json 依赖

```json
{
  "dependencies": ["BaseLib"]
}
```

## 添加新人物

### 1. 创建卡池

```csharp
public class MyCardPool : CustomCardPoolModel
{
    public override string Title => "mycharacter";
    public override string? TextEnergyIconPath => "res://mymod/images/energy_small.png";  // 24x24
    public override string? BigEnergyIconPath => "res://mymod/images/energy_big.png";      // 74x74
    public override Color DeckEntryCardColor => new(0.5f, 0.5f, 1f);
    public override bool IsColorless => false;
}

public class MyRelicPool : CustomRelicPoolModel
{
    public override string? TextEnergyIconPath => "res://mymod/images/energy_small.png";
    public override string? BigEnergyIconPath => "res://mymod/images/energy_big.png";
}

public class MyPotionPool : CustomPotionPoolModel
{
    public override string? TextEnergyIconPath => "res://mymod/images/energy_small.png";
    public override string? BigEnergyIconPath => "res://mymod/images/energy_big.png";
}
```

### 2. 创建人物

```csharp
public class MyCharacter : PlaceholderCharacterModel
{
    public override Color NameColor => new(0.5f, 0.5f, 1f);
    public override Color EnergyLabelOutlineColor => new(0.1f, 0.1f, 1f);
    public override CharacterGender Gender => CharacterGender.Masculine;
    public override int StartingHp => 80;
    
    // 场景路径
    public override string CustomVisualPath => "res://mymod/scenes/my_character.tscn";
    public override string CustomIconTexturePath => "res://mymod/images/icon.png";
    public override string CustomEnergyCounterPath => "res://mymod/scenes/my_energy_counter.tscn";
}
```

### 3. 创建卡池关联的卡牌

```csharp
[Pool(typeof(MyCardPool))]
public class MyStrike : CustomCardModel
{
    // 卡牌属性...
}
```

## 添加药水

```csharp
using BaseLib.Potions;

[Pool(typeof(IroncladPotionPool))]
public class MyPotion : CustomPotionModel
{
    public override PotionRarity Rarity => PotionRarity.Common;
    public override PotionSize Size => PotionSize.Small;
    
    public override void SetupDynamicVariables()
    {
        AddDynamicVariable(new HealVar(10));
    }
    
    public override IEnumerable<PotionAction> GetActions(PotionActionParams p)
    {
        yield return new HealAction(p.Player, GetDynamicVar<HealVar>());
    }
}
```

## 添加卡牌属性

### 常用 DynamicVar

| 类型 | 用途 | 示例 |
|------|------|------|
| `DamageVar` | 伤害 | `new DamageVar(6, 9)` |
| `BlockVar` | 格挡 | `new BlockVar(5, 8)` |
| `CardsVar` | 抽牌数 | `new CardsVar(2, 3)` |
| `EnergyVar` | 能量 | `new EnergyVar(1, 2)` |
| `HealVar` | 治疗 | `new HealVar(5, 8)` |
| `RepeatVar` | 重复次数 | `new RepeatVar(3, 4)` |
| `PowerVar<T>` | 能力层数 | `new PowerVar<StrengthPower>(2, 3)` |

### 常用 CardAction

| 类型 | 用途 |
|------|------|
| `DamageAction` | 造成伤害 |
| `BlockAction` | 获得格挡 |
| `DrawAction` | 抽牌 |
| `ApplyPowerAction` | 施加能力 |
| `GainEnergyAction` | 获得能量 |

## 添加充能球 (Orb)

```csharp
using BaseLib.Orbs;

public class MyOrb : CustomOrbModel
{
    public override string Id => "MY_ORB";
    
    public override void OnEvoke() { }
    public override void OnPassiveTrigger() { }
}
```

## 添加先古之民 (Ancient)

```csharp
using BaseLib.Ancients;

public class MyAncient : CustomAncientModel
{
    public override string Id => "MY_ANCIENT";
    // 配置先古属性...
}
```

## mod 联动

检测其他 mod 是否加载：

```csharp
if (ModLoader.IsModLoaded("OtherModId"))
{
    // 执行联动逻辑
}
```

## 局内保存

保存局内数据（战斗内持久化）：

```csharp
// 保存
RunSaveData.Set("mymod.mykey", value);

// 读取
var value = RunSaveData.Get<int>("mymod.mykey", defaultValue);
```
