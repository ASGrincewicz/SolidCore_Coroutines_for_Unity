# SolidCore Coroutines for Unity

Zero-allocation, struct-based coroutine scheduler for Unity — packaged as a standalone
Asset Store product. A single-threaded `CoroutineRunner` and a multi-threaded
`CoroutineJobSystem` tick thousands of coroutines per frame with no GC.

## What's inside

```
Runtime/Plugins/
  SolidCore.Coroutines.dll  (+ .xml)   ← the scheduler (netstandard2.1)
  SolidCore.Burst.dll       (+ .xml)   ← dependency: native instruction storage (BurstArray<T>)
  SolidCore.Collections.dll (+ .xml)   ← dependency: FastList<T> for the managed callback registry
```

Consumed as precompiled assemblies (`netstandard2.1`, the compatibility level Unity
loads managed plugins at). The `.xml` files provide inline API docs in the IDE. No
source, no `.asmdef` — the plugins are auto-referenced by your scripts.

## Install

Via Package Manager → Add package from git URL, or as a submodule under `Packages/`:

```
git submodule add git@github.com:ASGrincewicz/SolidCore_Coroutines_for_Unity.git Packages/com.solidcore.coroutines
```

## Quick start

```csharp
using SolidCore.Coroutines;

using var runner = CoroutineRunner.Create(expectedPeak: 10_000);

// Do(Action) and WaitUntil(Func<bool>) take ordinary managed delegates — IL2CPP/Mono friendly.
var seq = runner.Sequence()
    .Do(() => Debug.Log("step"))
    .WaitForSeconds(0.1f)
    .Do(() => Debug.Log("step"))
    .Build();

runner.StartAndRun(seq, count: 10_000);   // or runner.Tick(dt) each frame
```

## Updating the assemblies

Built from the [SolidCore](https://github.com/ASGrincewicz/SolidCore) repo:

```
dotnet build SolidCore.Coroutines/SolidCore.Coroutines.csproj -c Release
# The Coroutines build output also contains its dependency assemblies:
cp SolidCore.Coroutines/bin/Release/netstandard2.1/SolidCore.Coroutines.{dll,xml}  <this>/Runtime/Plugins/
cp SolidCore.Coroutines/bin/Release/netstandard2.1/SolidCore.Burst.{dll,xml}       <this>/Runtime/Plugins/
cp SolidCore.Coroutines/bin/Release/netstandard2.1/SolidCore.Collections.{dll,xml} <this>/Runtime/Plugins/
```
