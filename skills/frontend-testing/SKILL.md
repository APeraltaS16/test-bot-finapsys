---
name: frontend-testing
description: Apply Angular v18 unit testing practices with Karma and Jasmine to avoid flaky tests, wrong structure, and anti-patterns. Use when writing or reviewing frontend unit tests, when tests are flaky or failing intermittently, or when the user asks for test structure or testing best practices.
---

# Angular Frontend Testing

Use this skill whenever you write or review Angular unit tests so they stay stable, well-structured, and aligned with project standards. Stack: **Angular v18**, **Karma**, **Jasmine**.

## When to Apply

- Writing new `.spec.ts` files or modifying existing ones
- Reviewing or refactoring tests that are flaky or poorly structured
- User asks for "better tests", "fix flaky tests", or "test structure"
- Adding tests for components, services, or pipes

## Standards and references

**Align with the Angular v18 official testing guide** for setup, file location, and deeper topics:

- [Testing • Angular v18](https://v18.angular.dev/guide/testing) — overview, setup, and links to all testing guides.

The sections below cover **anti-patterns** and **flakiness** so the agent avoids them by default.

---

## Angular v18 official testing guidelines

Follow these conventions from the [Angular v18 testing guide](https://v18.angular.dev/guide/testing):

### Spec file name and location

- **Extension:** Use `.spec.ts` so tooling identifies spec files.
- **Unit specs:** Place the spec **next to the file it tests**, same folder. Same base name: `my.component.ts` → `my.component.spec.ts`. Keeps tests easy to find, shows coverage at a glance, and keeps spec and source in sync when moving/renaming.
- **Integration specs:** For tests that span multiple parts and don’t belong to one file, use a dedicated folder (e.g. `tests/`) and put specs there. Test helpers live next to their helper files.

### Running tests

In this project tests are run via yarn:

- **Local (watch):** `yarn run test` (or `yarn test`) — builds in watch mode and runs Karma.

### When to use which official sub-guide

| Need | Guide |
|------|--------|
| How much code is covered, thresholds | [Code coverage](https://v18.angular.dev/guide/testing/code-coverage) |
| Testing services | [Testing services](https://v18.angular.dev/guide/testing/services) |
| Component test basics | [Basics of testing components](https://v18.angular.dev/guide/testing/components-basics) |
| Component scenarios (inputs, async, etc.) | [Component testing scenarios](https://v18.angular.dev/guide/testing/components-scenarios) |
| Attribute directives | [Testing attribute directives](https://v18.angular.dev/guide/testing/attribute-directives) |
| Pipes | [Testing pipes](https://v18.angular.dev/guide/testing/pipes) |
| HTTP in tests | [HTTP Client – Testing](https://v18.angular.dev/guide/http/testing) |
| Test bugs / flakiness | [Debugging tests](https://v18.angular.dev/guide/testing/debugging) |
| TestBed and testing APIs | [Testing utility APIs](https://v18.angular.dev/guide/testing/utility-apis) |

Consult these guides when implementing or debugging tests for services, components, directives, pipes, or HTTP.

---

## Anti-Patterns to Avoid

### Structure

| Avoid | Prefer |
|-------|--------|
| Nested `describe` inside a method `describe` | One `describe` per method; multiple `it` blocks for scenarios |
| Vague names: `it('should work')`, `it('test form')` | `it('should [behavior] when [condition]')` |
| Testing `console.log` or `LoggerService` output | Do not assert on logger calls |
| Testing Angular lifecycle/change detection itself | Test only custom logic inside lifecycle hooks |

### Test Data

| Avoid | Prefer |
|-------|--------|
| `const mock = { id: 'x' } as IClaim` or manual DTOs | Factories from `@finapsys/shared/dist` (e.g. `CopaymentFactories.copaymentClaim.build({ ... })`) |
| Inline objects repeated in many tests | One factory-built object or shared mock at top of `describe` |

### Private Members and Types

| Avoid | Prefer |
|-------|--------|
| `(component as any).privateMethod()` | `component['privateMethod']()` (bracket notation) |

### Async and Timing

| Avoid | Prefer |
|-------|--------|
| Real delays or unmanaged timers | `fakeAsync` + `tick()` for debounce/timeouts |
| Leaving async work unresolved | `waitForAsync` or `fakeAsync` + `flush()` where needed |
| Skipping `fixture.detectChanges()` after state/async changes | Call `fixture.detectChanges()` after any change that should update the view |

### Isolation and Mocks

| Avoid | Prefer |
|-------|--------|
| Shared mutable state between tests | Reset in `beforeEach`: clear mocks, `jasmine.clock().uninstall?.(); jasmine.clock().install();` if using clocks |
| Real HTTP or unresolved requests | `HttpClientTestingModule` / `provideHttpClientTesting()` + `HttpTestingController`; call `req.flush(...)` and `httpMock.verify()` in `afterEach` when needed |
| Unresolved promises or observables | Use `of()`, `throwError()`, or `HttpTestingController` so each test completes deterministically |

### Assertions and Cleanup

| Avoid | Prefer |
|-------|--------|
| Only testing the happy path | Add tests for error paths with `throwError()` and assert on error handling |
| Forgetting to verify no outstanding HTTP requests | In tests that use HTTP mock: `afterEach(() => { httpMock?.verify(); });` |

---

## Quick Checklist Before Committing Tests

- [ ] One `describe` per method; no nested `describe` for methods
- [ ] Mocks and clocks reset in `beforeEach` where relevant
- [ ] Test data from `@finapsys/shared/dist` factories, not `as any` or ad-hoc objects
- [ ] Private methods accessed with `component['methodName']`, not `(component as any)`
- [ ] Async covered with `fakeAsync`/`tick()` or `waitForAsync`; `fixture.detectChanges()` after state/async changes
- [ ] HTTP tests use `HttpTestingController` and flush expectations; optional `httpMock.verify()` in `afterEach`
- [ ] Test names follow `'should [expected behavior] when [condition]'`
- [ ] No assertions on logger or framework-internal behavior
- [ ] Error scenarios tested where the component handles errors

---

## If Tests Are Already Flaky

1. **Timers**: Ensure any `setTimeout`/`debounceTime` is under `fakeAsync` and advance with `tick()` or `flush()`.
2. **HTTP**: Ensure every `expectOne` has a matching `flush()` and no request is left pending; add `httpMock.verify()` in `afterEach`.
3. **Order**: Ensure tests do not depend on execution order; reset mocks and component state in `beforeEach`.
4. **Change detection**: After setting inputs, signals, or async results, call `fixture.detectChanges()` before asserting on the DOM.

Iterate on this skill with the team when new anti-patterns or flakiness sources appear.
