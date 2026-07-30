# Upgrade Plan: matrix-agents-showcase (20260730044632)

- **Generated**: 2026-07-30 04:46:43 UTC
- **HEAD Branch**: main
- **HEAD Commit ID**: 3ee29a6

## Available Tools

**JDKs**
- JDK 21.0.11: /usr/lib/jvm/temurin-21-jdk-amd64/bin (current project JDK, used by Step 2 baseline)
- JDK 25.0.3: /usr/lib/jvm/temurin-25-jdk-amd64/bin (target JDK, used by Step 3+)

**Build Tools**
- Maven 3.9.16: /usr/share/apache-maven-3.9.16 (no wrapper present)

## Guidelines

> Note: Automatic flow — no user input required.

## Options

- Working branch: appmod/java-upgrade-20260730044632
- Run tests before and after the upgrade: true

## Upgrade Goals

- Java: 21 → 25

## Technology Stack

| Technology/Dependency    | Current  | Min Compatible | Why Incompatible                          |
|--------------------------|----------|----------------|-------------------------------------------|
| Java                     | 21       | 25             | User requested                            |
| Spring Boot              | 4.0.1    | Compatible     | Spring Boot 4.x supports Java 25          |
| Maven                    | 3.9.16   | Compatible     | Maven 3.9+ works with Java 25             |
| maven-compiler-plugin    | (SB managed) | 3.11+      | Managed by Spring Boot 4.0.1; should be compatible |
| Lombok                   | (SB managed) | Compatible | Works with Java 25                        |
| LangChain4j              | 1.14.0   | Compatible     | No Java version gating                    |
| Dockerfile               | java 21  | -              | Needs update to Java 25 base images       |

## Derived Upgrades

- Java 21 → 25 requires updating `<java.version>` property in pom.xml.
- Dockerfile base images updated from eclipse-temurin:21 to eclipse-temurin:25.
- maven build stage updated from maven:3.9-eclipse-temurin-21 to maven:3.9-eclipse-temurin-25.

## Impact Analysis

### Dependency Changes

| File     | Dependency   | Current | Action  | Target | Reason         |
|----------|-------------|---------|---------|--------|----------------|
| pom.xml  | java.version | 21      | upgrade | 25     | User requested |

### Source Code Changes

No source code changes required — no internal JDK API usages, no javax/jakarta migration issues (Spring Boot 4.x already uses Jakarta), no reflection into JDK internals detected.

### Configuration Changes

No application configuration changes required.

### CI/CD Changes

| File       | Location            | Current                            | Required Change                             |
|------------|--------------------|------------------------------------|---------------------------------------------|
| Dockerfile | Line 6 (FROM)       | maven:3.9-eclipse-temurin-21       | Change to: maven:3.9-eclipse-temurin-25     |
| Dockerfile | Line 30 (FROM)      | eclipse-temurin:21-jre-alpine      | Change to: eclipse-temurin:25-jre-alpine    |

### Risks & Warnings

- **No public LTS designation for Java 25**: Java 25 (GA September 2025) is a standard release; Java 21 is the current LTS. Java 25 is the next LTS candidate. The project will build and run on Java 25.
- **No test classes**: Project has no unit tests — compilation success is the only measurable criterion.

## Upgrade Steps

- Step 1: Setup Environment
  - **Rationale**: Verify JDK 25 is available
  - **Changes to Make**: None (JDK 25 already installed)
  - **Verification**: `echo $(/usr/lib/jvm/temurin-25-jdk-amd64/bin/java -version 2>&1)`, Expected: Java 25

- Step 2: Setup Baseline
  - **Rationale**: Record current build state with JDK 21
  - **Changes to Make**: None (compile-only check)
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-21-jdk-amd64 mvn clean test-compile -q`, Expected: SUCCESS

- Step 3: Upgrade Java Version to 25
  - **Rationale**: Update `<java.version>` in pom.xml and Dockerfile references
  - **Changes to Make**: All Dependency Changes and CI/CD Changes from Impact Analysis
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-25-jdk-amd64 mvn clean test-compile -q`, JDK 25, Expected: Compilation SUCCESS

- Step 4: CVE Validation & Fix
  - **Rationale**: Scan for known vulnerabilities in dependencies
  - **Changes to Make**: Fix any CVEs found
  - **Verification**: Re-run scan, Expected: No HIGH/CRITICAL CVEs or all patched

- Step 5: Final Validation
  - **Rationale**: Confirm all upgrade goals met with full build
  - **Changes to Make**: Resolve any remaining issues
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-25-jdk-amd64 mvn clean test -q`, JDK 25, Expected: BUILD SUCCESS
