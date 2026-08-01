# Upgrade Plan

## Overview

Upgrade the project to **Java 25** (latest LTS).

## Tasks

| # | Task | Type | Status |
|---|------|------|--------|
| 001 | Upgrade to Java 25 | upgrade | pending |

See `.metadata/tasks.json` for detailed task breakdown.

## Notes

- Only the JDK/Java version is being upgraded to Java 25.
- Spring Boot, Spring Framework, and Jakarta EE are not upgraded as part of this request.
- If your existing Spring Boot or Spring Framework version is incompatible with Java 25, consider also upgrading those frameworks (e.g., Spring Boot 3.5.x supports Java 25; Spring Boot 4.x requires Java 25).
