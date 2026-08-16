# Upgrade Plan

## Overview

Upgrade the **matrixagents** project to **Java 25**.

This plan targets a JDK-only upgrade to Java 25 (the latest LTS). Spring Boot, Spring Framework, and Jakarta EE are **not** upgraded as part of this plan per the explicit request scope.

> **Note**: If your project uses Spring Boot or Spring Framework versions that are incompatible with Java 25, you may need to upgrade those frameworks separately. Spring Boot 3.5.x+ and Spring Boot 4.x support Java 25. Please confirm if a framework upgrade is also desired.

## Tasks

See `.metadata/tasks.json` for the detailed task breakdown.

| ID | Description | Status |
|----|-------------|--------|
| 001-upgrade-java-25 | Upgrade to Java 25 | pending |

## Open Questions

- If the project uses Spring Boot versions older than 3.5.x or Spring Framework versions older than 6.x, those may be incompatible with Java 25. Would you like to also upgrade the framework(s) to a compatible version?
