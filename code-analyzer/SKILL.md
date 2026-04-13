---
name: "code-analyzer"
description: "Analyzes codebases comprehensively, extracting core modules and describing architecture. Invoke when user wants to understand a new project structure, analyze code functionality, or document module relationships."
---

# Code Analyzer

Performs comprehensive code analysis for understanding new codebases, extracting core functionality, and mapping module relationships.

## When to Use

- User opens a new project and wants to understand its purpose
- First-time code review or exploration
- Documenting existing codebase architecture
- Understanding complex module relationships
- Analyzing core implementation patterns

## Analysis Process

### 1. Project Overview

First, gather basic information:

- **Project type**: Identify the programming language, framework, and build system
- **Directory structure**: List main directories and their purposes
- **Entry points**: Find main files (main.c, index.js, app.py, etc.)
- **Configuration files**: Check for package.json, CMakeLists.txt, Makefile, etc.

### 2. Core Functionality Analysis

For each major component:

- **Purpose**: What does this module/component do?
- **Input/Output**: What data flows in and out?
- **Key algorithms**: Identify important algorithms or logic
- **Dependencies**: What does this component rely on?

### 3. Module Extraction

Identify and categorize:

**Core Modules**:
- Data structures and models
- Business logic implementations
- API endpoints or interfaces
- Utility functions

**Supporting Modules**:
- Configuration handling
- Error handling
- Logging and monitoring
- Testing utilities

**External Integrations**:
- Database connections
- API clients
- Third-party library usage

### 4. Module Relationship Mapping

Describe connections:

```
ModuleA --> ModuleB (uses)
ModuleB --> ModuleC (depends on)
ModuleD --> ModuleA, ModuleC (orchestrates)
```

Use ASCII diagrams to show:
- Data flow direction
- Control flow
- Shared data structures
- Interface boundaries

### 5. Documentation Template

Structure the analysis as:

```markdown
# Project Analysis Report

## 1. Project Overview
- **Project Name**: 
- **Type**: 
- **Purpose**: 
- **Language/Framework**: 

## 2. Core Functionality
### 2.1 Main Features
### 2.2 Key Algorithms

## 3. Module Architecture
### 3.1 Core Modules
### 3.2 Supporting Modules
### 3.3 External Dependencies

## 4. Module Relationships
[ASCII diagram showing connections]

## 5. Data Flow
[How data moves through the system]

## 6. Entry Points
[Where execution starts]

## 7. Configuration
[Key configuration files and their purposes]
```

## Implementation Guidelines

### Reading Files

1. Start with README or documentation files (if exist)
2. Identify entry points first
3. Trace execution flow from entry to core logic
4. Read header files for API definitions
5. Examine implementation details

### Analysis Techniques

1. **Breadth-first**: Get overview first, then dive deep
2. **Dependency analysis**: Draw import/include relationships
3. **Pattern recognition**: Identify common design patterns
4. **Interface extraction**: Find public APIs and their contracts
5. **Data structure mapping**: Understand how data is organized

### Module Relationship Description

When describing relationships, include:

- **Type of relationship**: uses, depends-on, implements, extends
- **Purpose of connection**: why these modules connect
- **Data exchanged**: what information flows between modules
- **Direction**: which module initiates the interaction

### Handling Large Codebases

For projects with many files:

1. Group related files by functionality
2. Analyze one feature area at a time
3. Build relationship map incrementally
4. Focus on core modules (usually 20% of code handles 80% of logic)
5. Identify abstraction layers and boundaries

## Example Analysis

For a simple C project structure:

```
project/
������ main.c          (entry point)
������ config.h       (configuration)
������ module_a.c/h    (core logic)
������ module_b.c/h    (utilities)
������ utils.c/h       (helper functions)
```

**Analysis**:

- **main.c**: Initializes program, calls core modules
- **module_a.c**: Implements primary business logic, depends on module_b
- **module_b.c**: Provides data processing utilities, uses utils
- **utils.c**: Common helper functions, no dependencies

**Relationship Map**:
```
main.c --> module_a.c --> module_b.c --> utils.c
```

## Output Requirements

1. **Language**: Use the same language as the user's query
2. **Depth**: Provide sufficient detail for understanding
3. **Clarity**: Use diagrams and examples to illustrate
4. **Completeness**: Cover all significant modules
5. **Accuracy**: Verify assumptions by reading actual code

## Validation

After analysis, confirm:

- All major components are documented
- Module relationships are accurate
- Core functionality is correctly identified
- User can understand the codebase from the analysis
