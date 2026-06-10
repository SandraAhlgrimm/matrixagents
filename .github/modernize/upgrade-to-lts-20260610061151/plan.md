# Upgrade Plan

## Overview

Upgrade the **matrixagents** project to **Java 25** (the latest LTS release) to benefit from modern language features, performance improvements, and long-term support.

## Tasks

See `.metadata/tasks.json` for the detailed task breakdown.

| # | Task ID | Description |
|---|---------|-------------|
| 1 | `001-upgrade-java-25` | Upgrade to Java 25 |

## Open Questions

> ⚠️ **Framework Compatibility Notice**
>
> Java 25 is only compatible with **Spring Boot 4.x** (and Spring Framework 7.x). Spring Boot 3.x supports up to Java 21 and is **not compatible** with Java 25.
>
> If your project uses Spring Boot 3.x or Spring Framework 6.x, upgrading to Java 25 will likely cause build or runtime failures.
>
> **Question**: Would you also like to upgrade Spring Boot to 4.x (and Spring Framework to 7.x) to ensure compatibility with Java 25? If yes, the plan will be updated to include a Spring Boot 4.x upgrade task, which also covers the javax.* → jakarta.* namespace migration.
