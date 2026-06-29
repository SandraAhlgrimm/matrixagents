# Upgrade Plan: matrix-agents-showcase (20260629062831)

- **Generated**: 2026-06-29
- **HEAD Branch**: main
- **HEAD Commit ID**: (git status)

## Available Tools

**JDKs**
- JDK 21: /usr/lib/jvm/temurin-21-jdk-amd64/bin (current project JDK, used by Step 2 baseline)
- JDK 25: /usr/lib/jvm/temurin-25-jdk-amd64/bin (target JDK, used by Steps 3+)

**Build Tools**
- Maven 3.9.16: /usr/share/apache-maven-3.9.16/bin (no wrapper present)

## Guidelines

> Upgrade JDK to Java 25. Do NOT upgrade Spring Boot, Spring Framework, or Jakarta EE.

## Options

- Working branch: appmod/java-upgrade-20260629062831
- Run tests before and after the upgrade: true

## Upgrade Goals

- Java: 21 → 25

## Technology Stack

| Technology/Dependency     | Current | Min Compatible | Why Incompatible                          |
|---------------------------|---------|----------------|-------------------------------------------|
| Java                      | 21      | 25             | User requested                            |
| Spring Boot               | 4.0.1   | -              | Already compatible with Java 25           |
| Maven                     | 3.9.16  | 3.9+           | Compatible                                |
| maven-compiler-plugin     | (managed by SB 4.0.1) | - | Compatible via Spring Boot parent |
| Dockerfile base image     | eclipse-temurin:21 | - | Must be updated to Java 25               |

## Derived Upgrades

- Java 21 → 25: Update `<java.version>` property in pom.xml
- Dockerfile: Update `maven:3.9-eclipse-temurin-21` → `maven:3.9-eclipse-temurin-25` and `eclipse-temurin:21-jre-alpine` → `eclipse-temurin:25-jre-alpine`

## Impact Analysis

### Dependency Changes

| File    | Dependency   | Current | Action  | Target | Reason         |
|---------|--------------|---------|---------|--------|----------------|
| pom.xml | java.version | 21      | upgrade | 25     | User requested |

### Source Code Changes

No source code changes required — no use of removed/internal APIs detected.

### Configuration Changes

No configuration changes required.

### CI/CD Changes

| File       | Location          | Current                          | Required Change                           |
|------------|-------------------|----------------------------------|-------------------------------------------|
| Dockerfile | Stage 2 FROM line | maven:3.9-eclipse-temurin-21     | Change to: maven:3.9-eclipse-temurin-25   |
| Dockerfile | Stage 3 FROM line | eclipse-temurin:21-jre-alpine    | Change to: eclipse-temurin:25-jre-alpine  |

### Risks & Warnings

- No test classes found in this project — test pass rate validation will be N/A (build-only verification).
- Spring Boot 4.0.1 is used; it supports Java 25. No framework incompatibilities expected.

## Upgrade Steps

- Step 1: Setup Environment
  - **Rationale**: Verify Java 25 JDK is available
  - **Changes to Make**: Confirm JDK 25 at /usr/lib/jvm/temurin-25-jdk-amd64/bin
  - **Verification**: `java -version`, Expected: Java 25

- Step 2: Setup Baseline
  - **Rationale**: Establish baseline build with Java 21
  - **Changes to Make**: None (read-only)
  - **Verification**: `mvn clean test-compile -q`, JDK 21, Expected: SUCCESS

- Step 3: Upgrade Java Version in pom.xml and Dockerfile
  - **Rationale**: Change all Java version references from 21 to 25
  - **Changes to Make**: Dependency Changes (java.version), CI/CD Changes (Dockerfile)
  - **Verification**: `mvn clean test-compile -q` with JDK 25, Expected: SUCCESS

- Step 4: CVE Validation & Fix
  - **Rationale**: Check for known CVEs in direct dependencies
  - **Changes to Make**: Fix any reported CVEs
  - **Verification**: Re-scan confirms no critical CVEs

- Step 5: Final Validation
  - **Rationale**: Full build and test verification with Java 25
  - **Changes to Make**: Fix any remaining issues
  - **Verification**: `mvn clean test -q` with JDK 25, Expected: All tests pass
