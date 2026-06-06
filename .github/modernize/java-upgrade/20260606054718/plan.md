# Upgrade Plan: matrix-agents-showcase (20260606054718)

- **Generated**: 2026-06-06 05:47:18
- **HEAD Branch**: main
- **HEAD Commit ID**: 3ee29a6

## Available Tools

**JDKs**
- JDK 21.0.11: /usr/lib/jvm/temurin-21-jdk-amd64/bin (current project JDK, used by step 2)
- JDK 25.0.3: /usr/lib/jvm/temurin-25-jdk-amd64/bin (target JDK, used by steps 3+)

**Build Tools**
- Maven 3.9 (system): mvn
- No Maven Wrapper present

## Guidelines

> Note: Fully autonomous upgrade — Java 21 → Java 25. No user input required.

## Options

- Working branch: appmod/java-upgrade-20260606054718
- Run tests before and after the upgrade: true

## Upgrade Goals

- **Java 25** (from Java 21)

## Technology Stack

| Technology/Dependency    | Current | Min Compatible | Why Incompatible |
|--------------------------|---------|----------------|------------------|
| Java                     | 21      | 25             | User requested   |
| Spring Boot              | 4.0.1   | 4.0.1          | Compatible with Java 25 |
| Maven                    | 3.9.x   | 3.9.0          | Compatible |
| maven-compiler-plugin    | (SB managed) | 3.11+ | Recommended for Java 25 support |
| Dockerfile (build stage) | eclipse-temurin:21 | eclipse-temurin:25 | Must match target JDK |
| Dockerfile (runtime)     | eclipse-temurin:21-jre-alpine | eclipse-temurin:25-jre-alpine | Must match target JDK |

## Derived Upgrades

- **Java 21 → 25**: Update `java.version` property in `pom.xml`
- **Dockerfile**: Update build and runtime base images to use JDK/JRE 25 (eclipse-temurin:25)

## Impact Analysis

### Dependency Changes

| File    | Dependency     | Current | Action  | Target | Reason           |
|---------|----------------|---------|---------|--------|------------------|
| pom.xml | java.version   | 21      | upgrade | 25     | User requested   |

### Source Code Changes

No source code changes required. The project uses standard Java APIs compatible with Java 25.

### Configuration Changes

No application configuration changes required.

### CI/CD Changes

| File       | Location             | Current                                | Required Change                         |
|------------|----------------------|----------------------------------------|-----------------------------------------|
| Dockerfile | Stage 2 FROM line    | maven:3.9-eclipse-temurin-21           | maven:3.9-eclipse-temurin-25           |
| Dockerfile | Stage 3 FROM line    | eclipse-temurin:21-jre-alpine         | eclipse-temurin:25-jre-alpine          |

### Risks & Warnings

- **Spring Boot 4.0.1 + Java 25**: Spring Boot 4.0.1 is a new release targeting Java 17+. Java 25 is supported. No breaking changes expected for this application's use of standard Spring Boot APIs.
- **LangChain4j beta versions**: `langchain4j-agentic` and `langchain4j-agentic-patterns` use beta24 releases. These should be compatible with Java 25 as they target standard JVM. Monitor for any reflection-based issues.

## Upgrade Steps

- Step 1: Setup Environment — Verify JDK 25 is available
  - **Rationale**: JDK 25 is already installed; verify it is accessible
  - **Changes to Make**: No file changes; verify JDK 25 path
  - **Verification**: `/usr/lib/jvm/temurin-25-jdk-amd64/bin/java -version`, Expected: Java 25

- Step 2: Setup Baseline — Compile and test with current JDK 21
  - **Rationale**: Establish baseline compilation and test pass rate
  - **Changes to Make**: None
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-21-jdk-amd64 mvn clean test-compile -q`, Expected: SUCCESS

- Step 3: Upgrade Java Version to 25
  - **Rationale**: Update java.version in pom.xml and Dockerfile base images to target Java 25
  - **Changes to Make**: 
    - Dependency Changes: java.version in pom.xml: 21 → 25
    - CI/CD Changes: Dockerfile build and runtime stages
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-25-jdk-amd64 mvn clean test-compile -q`, JDK 25, Expected: Compilation SUCCESS

- Step 4: Final Validation
  - **Rationale**: Confirm all upgrade goals met, full test suite passes with Java 25
  - **Changes to Make**: Fix any remaining issues found during validation
  - **Verification**: `JAVA_HOME=/usr/lib/jvm/temurin-25-jdk-amd64 mvn clean test -q`, JDK 25, Expected: 100% tests pass
