---
name: sts2-modding
description: "杀戮尖塔2 (Slay the Spire 2) 模组开发指南。提供环境配置、BaseLib接口（卡牌/遗物/药水/能力/人物）、Harmony补丁、场景节点结构、本地化BBCode等知识。当用户提及杀戮尖塔2、STS2、模组开发，或请求添加卡牌、遗物、人物等游戏内容时使用此skill。"
---

# 杀戮尖塔2 模组开发

## 版本信息

- **适用游戏版本**: public-beta 分支
- **引擎**: Godot 4.5.1 Mono
- **基础库**: BaseLib 0.2.1+

> 游戏处于抢先体验阶段，API 可能随版本更新变化，请查阅最新教程文档。

## 快速开始

### 环境配置

1. **安装 Godot 4.5.1 Mono**: [下载链接](https://godotengine.org/download/archive/4.5.1-stable/)（选择 .NET 版本）
2. **安装 .NET SDK 9+**: [下载链接](https://dotnet.microsoft.com/zh-cn/download)
3. **VS Code 插件**: C# Dev Kit、Godot Tools

### 模组文件结构

模组由三个文件组成，放置在 `游戏根目录/mods/{modid}/`：

| 文件 | 作用 | 生成方式 |
|------|------|----------|
| `{modid}.dll` | C# 代码 | `dotnet build` |
| `{modid}.pck` | 资源包 | Godot 导出 PCK |
| `{modid}.json` | 配置 | 手动编写 |

### 项目配置

**.csproj 配置**（关键部分）:
```xml
<Project Sdk="Godot.NET.Sdk/4.5.1">
  <PropertyGroup>
    <TargetFramework>net9.0</TargetFramework>
    <Sts2Dir>你的杀戮尖塔2目录</Sts2Dir>
    <Sts2DataDir>$(Sts2Dir)\data_sts2_windows_x86_64</Sts2DataDir>
  </PropertyGroup>
  <ItemGroup>
    <Reference Include="sts2">
      <HintPath>$(Sts2DataDir)\sts2.dll</HintPath>
      <Private>false</Private>
    </Reference>
    <Reference Include="0Harmony">
      <HintPath>$(Sts2DataDir)\0Harmony.dll</HintPath>
      <Private>false</Private>
    </Reference>
  </ItemGroup>
</Project>
```

**{modid}.json 配置**:
```json
{
  "id": "MyMod",
  "name": "我的模组",
  "author": "作者名",
  "version": "1.0",
  "has_pck": true,
  "has_dll": true,
  "dependencies": ["BaseLib"]
}
```

**Entry.cs 模板**:
```csharp
using Godot.Bridge;
using HarmonyLib;
using MegaCrit.Sts2.Core.Modding;

namespace MyMod.Scripts;

[ModInitializer("Init")]
public class Entry
{
    public static void Init()
    {
        new Harmony("sts2.author.mymod").PatchAll();
        ScriptManagerBridge.LookupScriptsInAssembly(typeof(Entry).Assembly);
    }
}
```

## BaseLib 接口

使用 BaseLib 添加新内容前，需要在 .csproj 中引用 BaseLib.dll，并在 json 中添加依赖。

### 添加卡牌

```csharp
using BaseLib.Cards;

[Pool(typeof(IroncladCardPool))]  // 指定卡池
public class MyCard : CustomCardModel
{
    public override int Cost => 1;
    public override CardRarity Rarity => CardRarity.Common;
    public override CardType Type => CardType.Attack;
    
    public override void SetupDynamicVariables()
    {
        AddDynamicVariable(new DamageVar(6, 9));  // 基础6，升级9
    }
    
    public override IEnumerable<CardAction> GetActions(CardActionParams p)
    {
        yield return new DamageAction(p.Target, GetDynamicVar<DamageVar>());
    }
}
```

### 添加遗物

```csharp
using BaseLib.Relics;

[Pool(typeof(IroncladRelicPool))]
public class MyRelic : CustomRelicModel
{
    public override RelicTier Tier => RelicTier.Common;
    public override string Id => "MY_RELIC";
    
    public override void OnEquip(Player player) { }
    public override void OnUnequip(Player player) { }
}
```

### 添加能力 (Power)

```csharp
using BaseLib.Powers;

public class MyPower : CustomPowerModel
{
    public override string Id => "MY_POWER";
    public override PowerType Type => PowerType.Buff;
    
    public override void OnApply() { }
    public override void AtTurnEnd() { }
}
```

## 详细参考文档

- 添加人物/卡池配置：见 [baselib-reference.md](baselib-reference.md)
- 场景节点结构规范：见 [scene-structure.md](scene-structure.md)
- 本地化与BBCode：见 [localization.md](localization.md)
- Harmony补丁模式：见 [harmony-patterns.md](harmony-patterns.md)
- 调试与问题解决：见 [debugging.md](debugging.md)

## 常用 Harmony 补丁

### 修改游戏行为

```csharp
[HarmonyPatch(typeof(TargetClass), nameof(TargetClass.MethodName))]
public static class MyPatch
{
    // 方法执行前
    static bool Prefix(TargetClass __instance) { return true; }
    
    // 方法执行后
    static void Postfix(TargetClass __instance, ref ReturnType __result) { }
}
```

### 替换卡图

```csharp
[HarmonyPatch(typeof(CardModel), nameof(CardModel.PortraitPath), MethodType.Getter)]
public static class CardPortrait_Patch
{
    private static readonly Dictionary<string, string> Portraits = new()
    {
        [nameof(StrikeIronclad)] = "res://mymod/images/strike.png",
    };
    
    static void Postfix(CardModel __instance, ref string __result)
    {
        if (Portraits.TryGetValue(__instance.GetType().Name, out var path))
            __result = path;
    }
}
```

## 调试方法

- **控制台**: 按 `~` 键打开，输入 `help` 查看命令
- **日志**: 修改 `launch_xxx.bat`，添加 `--log` 参数
- **联机测试**: 使用 `--fastmp=host` 和 `--fastmp=join -clientId=1001`

## 常见问题

| 问题 | 解决方案 |
|------|----------|
| 模组不加载 | 检查 json 格式、文件名一致性、dependencies |
| 场景加载失败 | 禁用"将文本资源转换为二进制" |
| 脚本找不到 | 确保调用了 `ScriptManagerBridge.LookupScriptsInAssembly()` |
| 存档丢失 | 模组存档在 `AppData\Roaming\SlayTheSpire2\steam\{steamid}\modded\` |
