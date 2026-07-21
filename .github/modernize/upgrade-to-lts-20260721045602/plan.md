# Upgrade Plan

## Overview

Upgrade the **matrixagents** project to **Java 25** (latest LTS).

## Tasks

| # | Task | Type | Status |
|---|------|------|--------|
| 001 | Upgrade to Java 25 | upgrade | pending |

See `.metadata/tasks.json` for detailed task breakdown.

## Task Details

### 001 — Upgrade to Java 25

Upgrade the JDK to Java 25. This includes:
- Updating build tooling (Maven/Gradle) compiler source/target/release settings to Java 25
- Updating runtime/container base images to Java 25
- Resolving any deprecated or removed APIs incompatible with Java 25

**Success Criteria**: Project builds successfully and all unit tests pass.

## Open Questions

> **Note**: This plan upgrades only the JDK to Java 25 as requested.  
> If the project uses Spring Boot or Spring Framework, and those versions are incompatible with Java 25, you may also need to upgrade them:
> - **Spring Boot 4.x** (requires Java 25) — compatible ✅
> - **Spring Boot 3.5.x** (supports Java 25) — compatible ✅
> - **Spring Boot 3.0.x–3.4.x** (max Java 21) — **incompatible** ⚠️
>
> If your project uses Spring Boot 3.0.x–3.4.x or older, please confirm whether you'd also like to upgrade Spring Boot to a Java 25-compatible version (3.5.x or 4.x).
