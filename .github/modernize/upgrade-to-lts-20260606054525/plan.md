# Upgrade Plan

## Overview

Upgrade the project to **Java 25** (latest LTS). This involves updating the JDK version, build tool configuration (Maven/Gradle compiler settings and toolchains), and ensuring all dependencies are compatible with Java 25.

## Tasks

See `.metadata/tasks.json` for the detailed task breakdown.

| # | Task | Type | Status |
|---|------|------|--------|
| 1 | Upgrade to Java 25 | upgrade | pending |

## Open Questions

> ⚠️ **Framework Compatibility Notice**
>
> Java 25 is only compatible with **Spring Boot 4.x** (which requires Spring Framework 7.x). Spring Boot 3.x supports up to Java 21.
>
> If your project uses **Spring Boot 3.x or earlier**, upgrading to Java 25 alone may cause build or runtime failures.
>
> **Do you also want to upgrade Spring Boot to 4.x and Spring Framework to 7.x to ensure full compatibility with Java 25?**
