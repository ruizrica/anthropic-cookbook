---
allowed-tools: Read(*),Write(*),Grep(*),Glob(*)
description: Identify code smells and refactor code following SOLID principles and design patterns
---

# Refactor Code Command

Identify code smells and refactor the specified files or modules.

**Code Smells to Detect**:
1. Long methods (> 50 lines)
2. Large classes (> 300 lines)
3. Duplicate code
4. Long parameter lists (> 4 params)
5. Primitive obsession
6. Feature envy
7. Data clumps

**Refactoring Patterns**:
- Extract Method
- Extract Class
- Replace Conditional with Polymorphism
- Introduce Parameter Object
- Replace Magic Numbers with Constants
- Simplify Conditional Expressions

Use the `refactoring-assistant` skill.

**Process**:
1. Analyze code for smells
2. Prioritize refactoring opportunities
3. Apply refactoring patterns
4. Ensure tests still pass
5. Document changes

**Output**:
- Updated code files
- Summary of refactorings applied
- Before/after comparisons
- Impact on code metrics (complexity, duplication)
