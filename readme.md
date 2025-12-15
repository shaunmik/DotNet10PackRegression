# .NET SDK Repro: `dotnet pack` Inconsistency (Net9 vs Net10)

This repository provides a minimal reproduction of a behavioral difference found when using the **.NET 10 SDK** (released November 2025) compared to the **.NET 9 SDK**.

## Description

When executing `dotnet pack`, the .NET 10 SDK **does not produce** a `.nupkg` file for the project transitively referencing NUnit3TestAdapter, while the .NET 9 SDK correctly produces the artifact.

Crucially, the `dotnet pack` command completes successfully without errors in both versions. It appears that under the .NET 10 SDK, the project is implicitly treated as `<IsPackable>false</IsPackable>` for this specific configuration, whereas in .NET 9 it remains packable by default.

## Project Structure

- **DotnetPackInconsistency**:
    - Contains a `PackageReference` to `NUnit3TestAdapter`.
- **DotnetPackInconsistency.Consumer**:
    - Contains a `ProjectReference` to `DotnetPackInconsistency`.

## Steps to Reproduce

### 1. Verify .NET 10 SDK behavior
```bash
dotnet new globaljson --sdk-version 10.0.101 --force
dotnet pack
# Observation: DotnetPackInconsistency.Consumer.nupkg IS NOT produced.
```

### 2. Verify .NET 9 SDK behavior
Run the following commands to force the use of the .NET 9 SDK:
```bash
dotnet new globaljson --sdk-version 9.0.308 --force
dotnet pack
# Observation: DotnetPackInconsistency.Consumer.1.0.0.nupkg is produced.
```
