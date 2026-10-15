# Setting Up Vitest with pnpm and TypeScript

Vitest is a lightweight testing framework that works well with TypeScript. Let's set it up and write a simple test for a function that adds two numbers.

## 1. Install Vitest

In your project directory, run:

```bash
pnpm add -D vitest
```

## 2. Add Test Scripts

Add the following scripts to your `package.json`:

```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest"
  }
}
```

Run `pnpm test` to execute tests once, or `pnpm test:watch` to rerun them when files change.

## 3. Create a Function

Create `src/add.ts`:

```ts
export function add(a: number, b: number): number {
  return a + b;
}
```

## 4. Write Your First Test

Create `src/add.test.ts`:

```ts
import { describe, expect, it } from "vitest";
import { add } from "./add";

describe("add", () => {
  it("adds two numbers", () => {
    expect(add(2, 3)).toBe(5);
  });

  it("handles negative numbers", () => {
    expect(add(-2, 3)).toBe(1);
  });

  it("returns zero when adding zero", () => {
    expect(add(0, 0)).toBe(0);
  });
});
```

Vitest automatically discovers files ending in `.test.ts` or `.spec.ts`.

## 5. Run the Tests

Execute:

```bash
pnpm test
```

Vitest should report three passing tests.

To run tests continuously while developing, use:

```bash
pnpm test:watch
```

## 6. Understand the Basics

A test typically has three parts:

- `describe()` groups related tests.
- `it()` defines one behavior to test.
- `expect()` checks whether the actual result matches the expected result.

For example:

```ts
expect(add(2, 3)).toBe(5);
```

This checks whether adding `2` and `3` produces `5`. If the result differs, the test fails.

That's it! Vitest is now configured, and you have a working example to build on as your project grows.
