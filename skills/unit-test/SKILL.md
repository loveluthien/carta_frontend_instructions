---
name: unit-test
description: write unit tests for a specific module or component in the CARTA frontend codebase.
---
# Unit Test Skill

This skill provides guidelines and commands for writing and running unit tests in the CARTA frontend codebase. Use Jest and React Testing Library.

## 1. Running Unit Tests

When you write or modify unit tests, you should run them to verify your implementation. Make sure necessary build steps are completed if you encounter missing module errors.

**Commands:**
- Run all tests (Jest automatically runs tests related to changed files by default): `npm test`
- Run a specific test file: `npm test -- path/to/file.test.ts`
- Run tests matching a specific name: `npm test -t "test name"`
- Run tests with verbose output: `npm test -- --verbose`

**Prerequisites (if tests fail due to missing WASM/Protobuf modules):**
1. Install dependencies: `npm install`
2. Build protobuf and WASM: `npm run build-protobuf && npm run build-libs && npm run build-wrappers`

## 2. Writing Unit Tests

Adhere to the following project-specific guidelines when writing unit tests:

### Directory Structure
Colocate test files directly alongside the source files they test, using the `.test.ts` or `.test.tsx` suffix.

```text
src/
  components/
    AComponent/
      AComponent.tsx
      AComponent.scss
      AComponent.test.tsx
  utilities/
    math/
      math.ts
      math.test.ts
```

### Test Structure
Use `describe` blocks to organize tests hierarchically.

```typescript
describe("[unit]", () => {
  test("[expected behavior]", () => {
    // test implementation
  });
  
  describe("[sub unit]", () => {
    test("[expected behavior]", () => {
      // test implementation
    });
  });
});
```

### Best Practices
- **Scope**: Focus on low-level unit tests for specific classes or functions.
- **Mocking**: Mock imported classes or functions with Jest when necessary to isolate components.
- **Imports**: Import TypeScript enums directly without using `index` files to avoid compile failures in tests.
- **React Components**:
  - Avoid mocking Blueprint.js objects to prevent complex setups.
  - Avoid snapshot testing to keep the codebase lean and intentional.
  - Follow [React Testing Library query priority](https://testing-library.com/docs/queries/about/#priority) when querying elements (e.g., prefer `getByRole`, `getByLabelText`, `getByText`).