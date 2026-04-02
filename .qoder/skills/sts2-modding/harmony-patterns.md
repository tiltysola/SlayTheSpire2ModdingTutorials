# Harmony 补丁模式

## 基础用法

### 初始化 Harmony

```csharp
using HarmonyLib;

[ModInitializer("Init")]
public class Entry
{
    public static void Init()
    {
        var harmony = new Harmony("sts2.author.modid");  // 唯一 ID
        harmony.PatchAll();  // 自动扫描并应用所有补丁
    }
}
```

### Prefix 补丁（方法执行前）

```csharp
[HarmonyPatch(typeof(TargetClass), nameof(TargetClass.MethodName))]
public static class MyPatch
{
    // 返回 false 可阻止原方法执行
    static bool Prefix(TargetClass __instance)
    {
        // 在原方法前执行
        return true;  // true = 继续执行原方法
    }
}
```

### Postfix 补丁（方法执行后）

```csharp
[HarmonyPatch(typeof(TargetClass), nameof(TargetClass.MethodName))]
public static class MyPatch
{
    static void Postfix(TargetClass __instance, ref ReturnType __result)
    {
        // 在原方法后执行
        // 可以修改 __result 改变返回值
    }
}
```

### 补丁 Getter/Setter

```csharp
// Getter
[HarmonyPatch(typeof(TargetClass), nameof(TargetClass.PropertyName), MethodType.Getter)]
public static class GetterPatch
{
    static void Postfix(ref string __result)
    {
        __result = "新值";
    }
}

// Setter
[HarmonyPatch(typeof(TargetClass), nameof(TargetClass.PropertyName), MethodType.Setter)]
public static class SetterPatch
{
    static void Prefix(ref string value)
    {
        // 修改要设置的值
    }
}
```

## 常用补丁示例

### 替换卡图

```csharp
[HarmonyPatch(typeof(CardModel), nameof(CardModel.PortraitPath), MethodType.Getter)]
public static class CardPortrait_Patch
{
    private static readonly Dictionary<string, string> CustomPortraits = new(StringComparer.OrdinalIgnoreCase)
    {
        [nameof(StrikeIronclad)] = "res://mymod/images/strike.png",
        [nameof(DefendIronclad)] = "res://mymod/images/defend.png",
    };

    static void Postfix(CardModel __instance, ref string __result)
    {
        var className = __instance?.GetType().Name;
        if (string.IsNullOrEmpty(className)) return;
        if (!CustomPortraits.TryGetValue(className, out var path)) return;
        if (!ResourceLoader.Exists(path)) return;
        __result = path;
    }
}
```

### 替换角色皮肤

```csharp
[HarmonyPatch(typeof(NCreature), nameof(NCreature._Ready))]
private static class NCreature_Ready_Patch
{
    private const string TargetCharacterId = "IRONCLAD";
    private const string SkinPath = "res://mymod/spineskins/my_skin.tres";
    private static Resource? _skinData;

    static NCreature_Ready_Patch()
    {
        _skinData = ResourceLoader.Load<Resource>(SkinPath);
    }

    static void Postfix(NCreature __instance)
    {
        if (_skinData == null) return;
        
        var player = __instance?.Entity?.Player;
        if (player == null) return;
        
        if (!string.Equals(player.Character.Id.Entry, TargetCharacterId, StringComparison.OrdinalIgnoreCase))
            return;
        
        var visuals = __instance.Visuals;
        if (visuals?.SpineBody == null || !visuals.HasSpineAnimation) return;
        
        new MegaSprite(visuals.SpineBody).SetSkeletonDataRes(new MegaSkeletonDataResource(_skinData));
    }
}
```

### 替换任意模型

```csharp
[HarmonyPatch(typeof(CharacterModel), nameof(CharacterModel.CreateVisuals))]
public static class CharacterVisuals_Patch
{
    static void Postfix(CharacterModel __instance, ref NCreatureVisuals __result)
    {
        if (__instance.Id.Entry != "TARGET_CHARACTER") return;
        
        var scene = ResourceLoader.Load<PackedScene>("res://mymod/scenes/my_character.tscn");
        __result = scene.Instantiate<NCreatureVisuals>();
    }
}
```

## Transpiler 补丁

用于修改方法的 IL 代码（高级用法）。

```csharp
[HarmonyPatch(typeof(TargetClass), nameof(TargetClass.MethodName))]
public static class MyTranspiler
{
    static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions)
    {
        var codes = new List<CodeInstruction>(instructions);
        // 修改 IL 代码
        return codes;
    }
}
```

## 反射访问私有成员

使用 Harmony 的 Traverse API：

```csharp
// 读取私有字段
var value = Traverse.Create(instance).Field("_privateField").GetValue<int>();

// 设置私有字段
Traverse.Create(instance).Field("_privateField").SetValue(newValue);

// 调用私有方法
Traverse.Create(instance).Method("PrivateMethod", arg1, arg2).GetValue();
```

## 补丁优先级

```csharp
[HarmonyPatch(typeof(TargetClass), nameof(TargetClass.MethodName))]
[HarmonyPriority(Priority.High)]  // 优先执行
public static class HighPriorityPatch { }

[HarmonyPatch(typeof(TargetClass), nameof(TargetClass.MethodName))]
[HarmonyPriority(Priority.Low)]   // 后执行
public static class LowPriorityPatch { }
```

优先级常量：
- `Priority.First` = 0
- `Priority.VeryHigh` = 100
- `Priority.High` = 200
- `Priority.Normal` = 400 (默认)
- `Priority.Low` = 600
- `Priority.VeryLow` = 800
- `Priority.Last` = 999

## 条件补丁

```csharp
[HarmonyPatch]
public static class ConditionalPatch
{
    // 动态确定要补丁的方法
    static MethodBase TargetMethod()
    {
        return AccessTools.Method(typeof(TargetClass), "MethodName");
    }
    
    // 决定是否应用补丁
    static bool Prepare()
    {
        return SomeCondition;  // false = 不应用补丁
    }
}
```

## 参考文档

Harmony 官方文档: https://harmony.pardeike.net/articles/basics.html
