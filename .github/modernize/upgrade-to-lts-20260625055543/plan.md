# Java Upgrade Plan

## Overview

Upgrade the project to **Java 25** (latest LTS), bringing long-term support, modern language features, performance improvements, and security enhancements.

## Tasks

See `.metadata/tasks.json` for the detailed task breakdown.

| # | Task ID | Description | Type | Status |
|---|---------|-------------|------|--------|
| 1 | 001-upgrade-java-25 | Upgrade JDK to Java 25 | upgrade | pending |

## Open Questions

> **Note — Spring Boot / Spring Framework compatibility**: This plan upgrades the JDK to Java 25 only, without touching Spring Boot or Spring Framework. If your project uses Spring Boot 3.0.x–3.4.x (which supports up to Java 21) or an older version, it may not be compatible with Java 25. Spring Boot 3.5.x supports Java 25, and Spring Boot 4.x requires Java 25. Please confirm whether you also want to upgrade Spring Boot and/or Spring Framework to a compatible version.
