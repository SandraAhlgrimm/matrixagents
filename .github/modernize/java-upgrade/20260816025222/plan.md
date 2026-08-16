# Upgrade Plan: matrix-agents-showcase (20260816025222)

- **Generated**: 2026-08-16 02:52:22
- **HEAD Branch**: main
- **HEAD Commit ID**: 3ee29a6

## Available Tools

**JDKs**
- JDK 17: /usr/lib/jvm/temurin-17-jdk-amd64 (current default JDK)
- JDK 21: /usr/lib/jvm/temurin-21-jdk-amd64 (current project JDK, used by Step 2 baseline)
- JDK 25: /usr/lib/jvm/temurin-25-jdk-amd64 (target JDK, used by Steps 3+)

**Build Tools**
- Maven 3.9.16: /usr/bin/mvn (compatible with Java 25)

## Guidelines

> Note: Upgrade JDK to Java 25 only. Do not upgrade Spring Boot, Spring Framework, or Jakarta EE.

## Options

- Working branch: appmod/java-upgrade-20260816025222
- Run tests before and after the upgrade: true

## Upgrade Goals

- Java: 21 → 25

## Technology Stack

| Technology/Dependency          | Current | Min Compatible | Why Incompatible                          |
|-------------------------------|---------|----------------|-------------------------------------------|
| Java                           | 21      | 25             | User requested upgrade                    |
| Spring Boot                    | 4.0.1   | (no change)    | Already compatible with Java 25           |
| Maven                          | 3.9.16  | 3.9+           | Already compatible                        |
| Lombok                         | managed | 1.18.24+       | Should be compatible with Java 25         |
| LangChain4j                    | 1.14.0  | (no change)    | No Java 25 incompatibilities expected     |

## Derived Upgrades

- Java 21 → 25: Update `<java.version>` property in pom.xml from `21` to `25`
- Dockerfile: Update base images from `eclipse-temurin:21` to `eclipse-temurin:25`

## Impact Analysis

### Dependency Changes

| File    | Dependency    | Current | Action  | Target | Reason          |
|---------|---------------|---------|---------|--------|-----------------|
| pom.xml | java.version  | 21      | upgrade | 25     | User requested  |

### Source Code Changes

No source code changes required — no deprecated/removed APIs identified that affect this codebase when moving from Java 21 to Java 25.

### Configuration Changes

No application configuration changes required.

### CI/CD Changes

| File       | Location           | Current                          | Required Change                          |
|------------|--------------------|----------------------------------|------------------------------------------|
| Dockerfile | Stage 2 FROM line  | maven:3.9-eclipse-temurin-21     | Change to: maven:3.9-eclipse-temurin-25  |
| Dockerfile | Stage 3 FROM line  | eclipse-temurin:21-jre-alpine    | Change to: eclipse-temurin:25-jre-alpine |

### Risks & Warnings

- **No test classes present**: The project has no test source files. Compilation success serves as the primary validation.
- **Dockerfile base image update**: The `eclipse-temurin:25-jre-alpine` image must be available on Docker Hub. Maven's official image also needs to support JDK 25 (`maven:3.9-eclipse-temurin-25`).

## Upgrade Steps

- Step 1: Setup Environment
  - **Rationale**: Verify JDK 25 is available (already confirmed at /usr/lib/jvm/temurin-25-jdk-amd64)
  - **Changes to Make**: None; JDK 25 is already installed
  - **Verification**: `/usr/lib/jvm/temurin-25-jdk-amd64/bin/java -version`, Expected: Java 25

- Step 2: Setup Baseline
  - **Rationale**: Establish baseline compilation with current JDK 21 before upgrading
  - **Changes to Make**: None
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-21-jdk-amd64 mvn clean test-compile -q`, JDK 21, Expected: Compilation SUCCESS

- Step 3: Upgrade pom.xml to Java 25 and Update Dockerfile
  - **Rationale**: Update all Java version references to 25
  - **Changes to Make**: 
    - Dependency Changes: `java.version` 21→25 in pom.xml
    - CI/CD Changes: Dockerfile base images updated to temurin-25
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-25-jdk-amd64 mvn clean test-compile -q`, JDK 25, Expected: Compilation SUCCESS

- Step 4: CVE Validation & Fix
  - **Rationale**: Scan direct dependencies for known CVEs and fix any found
  - **Changes to Make**: Upgrade vulnerable dependency versions as needed
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-25-jdk-amd64 mvn clean test-compile -q`, Expected: Compilation SUCCESS

- Step 5: Final Validation
  - **Rationale**: Confirm all upgrade goals are met with clean build and full test suite
  - **Changes to Make**: Fix any remaining issues
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-25-jdk-amd64 mvn clean test -q`, JDK 25, Expected: BUILD SUCCESS
