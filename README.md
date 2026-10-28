# Agents

This project contains a collection of coding standards and best practice documents, aimed at providing uniform coding standards for the development team.

## Project Introduction

This is a knowledge base focused on code standards, incorporating C# programming norms and Git commit conventions to help teams maintain consistent code style and improve code quality.

## Directory Structure

```
├── CSharp/
│   ├── CSharp.md           # C# Coding Standards (English)
│   └── CSharp.zh-CN.md     # C# Coding Standards (Chinese)
└── Git/
    ├── git-commit/
    │   └── SKILL.md        # Conventional Commits Specification (English)
    └── git-commit-zh-cn/
        └── SKILL.md        # Conventional Commits Specification (Chinese)
```

## Content Overview

### C# Coding Standards

Covers the following topics:

- **Priority** - Priority level of standards
- **Format** - Code formatting rules
- **Naming** - Naming conventions
- **Types and Members** - Type and member design
- **Syntax and Language Features** - C# language features and syntax best practices
- **Exception Handling** - Exception handling specifications
- **Async** - Asynchronous programming specifications
- **Strings** - String handling
- **Comments** - Commenting standards
- **Editing Existing Code** - Guidance for modifying existing code

### Git Commit Conventions

Based on the [Conventional Commits 1.0.0](https://www.conventionalcommits.org/) specification, includes:

- Commit message structure
- Mapping between commit types and version numbers
- Marking methods for breaking changes
- Multiple commit message examples

## Usage Recommendations

- Developers newly joining the team should first read the relevant specification documents
- Follow the conventions in this specification when writing code
- Write commit messages according to the Conventional Commits format when submitting code