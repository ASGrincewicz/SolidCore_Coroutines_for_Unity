# SolidCore Coroutines for Unity

Zero-allocation, struct-based coroutine scheduler for Unity — packaged as a standalone
Asset Store product. A single-threaded `CoroutineRunner` and a multi-threaded
`CoroutineJobSystem` tick thousands of coroutines per frame with no GC.

## What's inside

```
Runtime/Plugins/
  SolidCore.Coroutines.dll (+ .xml)   ← the scheduler (netstandard2.1)
  SolidCore.Burst.dll      (+ .xml)   ← its only dependency (unmanaged collections)
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
var seq = runner.Sequence().Do(&Step).WaitForSeconds(0.1f).Do(&Step).Build();
runner.StartAndRun(seq, count: 10_000);   // or runner.Tick(dt) each frame
```

## Updating the assemblies

Built from the [SolidCore](https://github.com/ASGrincewicz/SolidCore) repo:

```
dotnet build SolidCore.Coroutines/SolidCore.Coroutines.csproj -c Release
cp SolidCore.Coroutines/bin/Release/netstandard2.1/SolidCore.Coroutines.{dll,xml} <this>/Runtime/Plugins/
cp SolidCore.Burst/bin/Release/netstandard2.1/SolidCore.Burst.{dll,xml}           <this>/Runtime/Plugins/
```
