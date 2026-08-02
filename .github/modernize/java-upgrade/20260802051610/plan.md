# Upgrade Plan: matrix-agents-showcase (20260802051610)

- **Generated**: 2026-08-02 05:16:10
- **HEAD Branch**: main
- **HEAD Commit ID**: 3ee29a6

## Available Tools

**JDKs**
- JDK 21.0.11: /usr/lib/jvm/temurin-21-jdk-amd64/bin (current project JDK, used for baseline in Step 2)
- JDK 25.0.3: /usr/lib/jvm/temurin-25-jdk-amd64/bin (target JDK, used from Step 3 onward)

**Build Tools**
- Maven 3.9.16: /usr/share/apache-maven-3.9.16/bin/mvn (system Maven; no wrapper present)

## Guidelines

> Note: Upgrade JDK from Java 21 to Java 25. Update build configuration to target Java 25 source/target compatibility. Ensure all dependencies are compatible with Java 25. Fix any compilation issues or deprecated API usages.

## Options

- Working branch: appmod/java-upgrade-20260802051610
- Run tests before and after the upgrade: true

## Upgrade Goals

- Java: 21 → **25**

## Technology Stack

| Technology/Dependency        | Current | Min Compatible | Why Incompatible                              |
|------------------------------|---------|----------------|-----------------------------------------------|
| Java                         | 21      | 25             | User requested upgrade to Java 25             |
| Spring Boot                  | 4.0.1   | 4.0.x          | Spring Boot 4.x supports Java 17+; compatible |
| Maven                        | 3.9.16  | 3.9+           | Compatible; no upgrade needed                 |
| langchain4j                  | 1.14.0  | any            | Pure Java library; compatible with Java 25    |
| langchain4j-agentic          | 1.14.0-beta24 | any      | Compatible with Java 25                       |
| langchain4j-open-ai-official | 1.14.0-beta24 | any      | Compatible with Java 25                       |
| azure-identity               | 1.14.2  | any            | Compatible with Java 25                       |
| lombok                       | managed | 1.18.20+       | Compatible with Java 25                       |

## Derived Upgrades

- **Dockerfile**: Update `maven:3.9-eclipse-temurin-21` → `maven:3.9-eclipse-temurin-25` and `eclipse-temurin:21-jre-alpine` → `eclipse-temurin:25-jre-alpine` to match the new JDK target.
- No Kotlin present; no Kotlin upgrade needed.
- Maven 3.9+ already satisfies Java 25 build requirements.

## Impact Analysis

### Dependency Changes

| File    | Dependency   | Current | Action  | Target | Reason                      |
|---------|-------------|---------|---------|--------|-----------------------------|
| pom.xml | java.version | 21      | upgrade | 25     | User requested              |

### Source Code Changes

No source code changes required. The codebase uses standard Java APIs compatible with Java 25. No removed/deprecated APIs detected.

### Configuration Changes

No application configuration changes needed.

### CI/CD Changes

| File       | Location | Current                              | Required Change                              |
|------------|----------|--------------------------------------|----------------------------------------------|
| Dockerfile | line 12  | maven:3.9-eclipse-temurin-21         | Change to: maven:3.9-eclipse-temurin-25      |
| Dockerfile | line 27  | eclipse-temurin:21-jre-alpine        | Change to: eclipse-temurin:25-jre-alpine     |

### Risks & Warnings

- **No unit tests present**: The project has no test classes under `src/test`. The Final Validation will verify compilation only. No test coverage risk for this upgrade.
- **Spring Boot 4.0.1 with Java 25**: Spring Boot 4.x officially targets Java 17+ LTS versions; Java 25 (non-LTS) should compile and run correctly with Spring Boot 4.x.

## Upgrade Steps

- Step 1: Setup Environment
  - **Rationale**: Verify JDK 25 is available for the upgrade
  - **Changes to Make**: None — JDK 25 already installed at /usr/lib/jvm/temurin-25-jdk-amd64/bin
  - **Verification**: `java -version` with JDK 25, Expected: JDK 25.0.3

- Step 2: Setup Baseline
  - **Rationale**: Record current build status using JDK 21 before making changes
  - **Changes to Make**: None — compile and test with current JDK 21
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-21-jdk-amd64 mvn clean compile test-compile -q`, Expected: SUCCESS

- Step 3: Upgrade Java Version to 25
  - **Rationale**: Update the java.version property and Dockerfile to target Java 25
  - **Changes to Make**:
    - pom.xml: `<java.version>21</java.version>` → `<java.version>25</java.version>`
    - Dockerfile line 12: `maven:3.9-eclipse-temurin-21` → `maven:3.9-eclipse-temurin-25`
    - Dockerfile line 27: `eclipse-temurin:21-jre-alpine` → `eclipse-temurin:25-jre-alpine`
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-25-jdk-amd64 mvn clean test-compile -q`, Expected: SUCCESS

- Step 4: CVE Validation & Fix
  - **Rationale**: Scan all direct dependencies for known CVEs and fix any found
  - **Changes to Make**: Fix any CVEs found by `#validate-cves-for-java`
  - **Verification**: Re-scan shows no critical CVEs unresolved

- Step 5: Final Validation
  - **Rationale**: Confirm all upgrade goals are met with a clean build and test run
  - **Changes to Make**: Fix any remaining issues
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-25-jdk-amd64 mvn clean test -q`, Expected: BUILD SUCCESS
