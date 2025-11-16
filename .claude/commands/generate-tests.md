---
allowed-tools: Read(*),Write(*),Glob(*)
description: Generate comprehensive unit and integration tests for specified files or modules
---

# Generate Tests Command

Generate comprehensive tests for the specified files or modules.

**Steps**:
1. Read the source files
2. Identify functions, classes, and methods to test
3. Generate unit tests covering:
   - Happy path
   - Edge cases (null, undefined, empty values)
   - Error conditions
   - Boundary conditions

4. Generate integration tests for:
   - API endpoints
   - Database operations
   - External service interactions

Use the `unit-test-generator` and `integration-test-builder` skills.

**Test Framework**: Automatically detect (Jest, Vitest, pytest, etc.) from package.json or requirements.txt

**Output**: Create test files following naming convention:
- `filename.test.ts` or `filename.spec.ts` for JavaScript/TypeScript
- `test_filename.py` for Python
- `filename_test.go` for Go

Include:
- Proper test structure (describe/it blocks)
- Mocks for external dependencies
- Fixtures for test data
- Clear test descriptions
