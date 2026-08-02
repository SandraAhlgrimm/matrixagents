# Upgrade Plan

## Overview

Upgrade the **matrixagents** project to **Java 25** (latest LTS).

This plan covers upgrading the JDK to Java 25, updating build configuration for the new source/target compatibility level, and ensuring all dependencies and code are compatible with Java 25.

> **Note**: This plan upgrades only the JDK. Spring Boot, Spring Framework, and Jakarta EE are not modified. If your existing Spring Boot or Spring Framework versions are incompatible with Java 25, consider also upgrading those frameworks.

## Tasks

See [.metadata/tasks.json](.metadata/tasks.json) for the detailed task breakdown.

| # | Task | Type | Status |
|---|------|------|--------|
| 001 | Upgrade JDK to Java 25 | upgrade | pending |

## Open Questions

- If the project uses Spring Boot 3.0.x–3.4.x (which supports up to Java 21), it will need to be upgraded to Spring Boot 3.5.x or 4.x to remain compatible with Java 25. Would you like to also upgrade Spring Boot as part of this plan?
