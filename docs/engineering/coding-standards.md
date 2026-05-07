# Coding Standards

These standards apply to all server-side code written in C# and .NET across Condado engagements. The goal is a codebase that any developer on our team can pick up, navigate, and extend without guesswork.

Standards are enforced automatically where possible (analyzers, `.editorconfig`, CI). Where they require judgment, this document provides the principle and enough examples to apply it consistently.

When citing a rule in a pull request or code review, use the section code in brackets (e.g., `[CS-NAME-03]`). Codes are stable.

---

## 1. Project Structure [CS-PROJ]

### 1.1 Solution Layout [CS-PROJ-01]

Every .NET solution follows this structure:

```
<Solution>.sln
.editorconfig
Directory.Build.props         ← shared MSBuild settings
Directory.Packages.props      ← central package versions
src/
  <Product>.Api/              ← web API host
  <Product>.Application/     ← use cases, application services
  <Product>.Domain/          ← entities, domain logic
  <Product>.Infrastructure/  ← EF Core, external integrations
tests/
  <Product>.UnitTests/
  <Product>.IntegrationTests/
```

Small services can collapse layers (merging Application and Domain is fine). Don't add layers without a clear reason.

### 1.2 File and Namespace Rules [CS-PROJ-02]

- One public type per file; filename matches the type name
- Namespaces match folder structure, PascalCase, starting with `CompanyName.<Service>.<Feature>`
- Use file-scoped namespaces (`namespace X;` at top of file)
- Group files by feature, not by technical layer — avoid `Helpers/`, `Utilities/`, or `Common/` dumping grounds

### 1.3 Project-Wide Configuration [CS-PROJ-03]

Every solution includes `.editorconfig`, `Directory.Build.props`, and `Directory.Packages.props`. `TreatWarningsAsErrors` is `true` for all production projects. Warnings are defects.

---

## 2. Naming Conventions [CS-NAME]

### 2.1 General Rules [CS-NAME-01]

| Element | Convention | Example |
|---|---|---|
| Class | PascalCase | `ArchiveService` |
| Interface | `I` + PascalCase | `IArchiveService` |
| Method | PascalCase | `ArchiveRecordingAsync` |
| Property | PascalCase | `RecordingId` |
| Local variable | camelCase | `recordingId` |
| Parameter | camelCase | `recordingId` |
| Private field | `_` + camelCase | `_repository` |
| Constant | PascalCase | `MaxRetryCount` |
| Enum | PascalCase, singular | `RecordingStatus` |
| Enum member | PascalCase | `Archived` |

No `ALL_CAPS` in C# — not even for constants.

### 2.2 Meaningful Names [CS-NAME-02]

- Names describe intent, not type or implementation
- Avoid abbreviations unless industry-standard (`URL`, `HTTP`, `Id`)
- Boolean names read as questions: `IsArchived`, `HasPermission`, `CanExport`
- No Hungarian notation (`strName`, `iCount`)
- Pick one word for one concept and use it everywhere — don't mix "customer" and "client" for the same domain entity

### 2.3 Async Suffix [CS-NAME-03]

Every method returning `Task`, `Task<T>`, `ValueTask`, or `IAsyncEnumerable<T>` ends with `Async`. Interface methods and implementations must match.

### 2.4 Test Method Names [CS-NAME-04]

Follow the `MethodUnderTest_Scenario_ExpectedResult` pattern:

```
ArchiveRecordingAsync_WhenRecordingMissing_ReturnsNotFound
CalculateRetention_WhenPolicyIsLegalHold_ReturnsMaxDuration
```

---

## 3. Code Style [CS-STYLE]

Formatting is enforced by `.editorconfig` and `dotnet format` on every CI build. Reviewers don't debate whitespace.

### 3.1 Baseline Rules [CS-STYLE-01]

- Four-space indentation, no tabs
- Allman braces — opening brace on its own line
- Braces required on every block, even single-statement `if`/`for`/`foreach`/`while`
- One statement per line; one declaration per line
- Line length soft limit: 120 characters
- `using` directives outside the namespace; System namespaces first, then third-party, then project namespaces

### 3.2 var vs. Explicit Type [CS-STYLE-02]

Use `var` when the type is obvious from the right side: `var customer = new Customer();`

Use the explicit type when the right side doesn't reveal it: `Customer customer = repo.GetById(id);`

Never use `var` when it hides a meaningful type distinction (e.g., `IEnumerable<T>` vs. `List<T>` where the difference matters).

### 3.3 Access Modifiers [CS-STYLE-03]

Always specify access modifiers — don't rely on defaults. Prefer `private` over `internal`. Widen access only when necessary. Mark concrete classes `sealed` by default unless designed for inheritance. Fields are `readonly` by default; mutable state requires justification.

---

## 4. Language Usage [CS-LANG]

Prefer idiomatic, modern C#. Avoid clever code.

### 4.1 Use Modern Features [CS-LANG-01]

Prefer these when they improve clarity:

- Pattern matching for type-based branching
- Switch expressions for multi-branch mapping
- Records for immutable data carriers
- `nameof()` instead of string literals for member names
- String interpolation instead of `string.Format`
- Collection expressions `[1, 2, 3]` for initialization

### 4.2 Anti-Patterns to Avoid [CS-LANG-02]

- Magic numbers and magic strings — use named constants or enums
- Boolean parameters for flow control — prefer enums or separate methods
- More than 4–5 parameters — use a parameter object or record
- `out` parameters except in `TryParse`-style patterns
- `static` mutable state outside of configuration or caches

### 4.3 Collections [CS-LANG-03]

- Return `IReadOnlyList<T>` or `IReadOnlyCollection<T>` from public methods
- Accept `IEnumerable<T>` for public methods that consume collections
- Never return `null` — return an empty collection instead
- Never use `ArrayList`, `Hashtable`, or other non-generic legacy collections
- LINQ: use method syntax (`.Where().Select()`); materialize intentionally with `.ToList()` only when you need it

---

## 5. Null Handling [CS-NULL]

Nullable reference types are enabled project-wide. Null is a design decision, not an accident.

- A reference type without `?` means non-null always
- A reference type with `?` means may be null — document the meaning
- Suppress nullable warnings with `!` only with a comment explaining why
- Use `ArgumentNullException.ThrowIfNull(parameter)` at entry points for public and protected methods
- Never silently substitute defaults for null input — throw or return a meaningful result type

---

## 6. Async and Threading [CS-ASYNC]

### 6.1 Core Rules [CS-ASYNC-01]

- Every I/O-bound operation (database, network, file) exposes an async signature
- Never call `.Result`, `.Wait()`, or `.GetAwaiter().GetResult()` on async work in application code
- `async void` is only for event handlers — never for normal methods
- Every async method accepts a `CancellationToken` as its last parameter (defaulting to `default`) and passes it downstream

### 6.2 Parallel Work [CS-ASYNC-02]

- Use `Task.WhenAll` for independent async operations running concurrently
- Bound concurrency — unbounded fan-out exhausts connections and threads
- Never use `Thread.Sleep` in async code; use `Task.Delay`
- Lock on a dedicated `private readonly object _lock`, never on `this`, `typeof(X)`, strings, or public objects

---

## 7. Error Handling [CS-ERR]

### 7.1 Throwing [CS-ERR-01]

- Throw specific exception types — avoid `throw new Exception(...)`
- Use built-in exceptions when they fit (`ArgumentException`, `InvalidOperationException`)
- Create custom exceptions only when they carry meaningfully different semantics
- Custom exception names end with `Exception`: `RecordingNotFoundException`

### 7.2 Catching [CS-ERR-02]

- Catch the most specific type you can actually handle
- Don't catch `Exception` except at top-level handlers (global middleware)
- Never catch and swallow — at minimum, log with full context
- Rethrow with `throw;`, not `throw ex;` — preserve the stack trace

### 7.3 Exceptions vs. Results [CS-ERR-03]

Exceptions are for unexpected, exceptional conditions. Use result types or `TryGet` patterns for expected outcomes that can fail: "not found" is a result (HTTP 404), not an exception. Validation failure is a result (HTTP 400), not an exception.

---

## 8. Logging [CS-LOG]

Logs are structured data — they get queried and alerted on.

### 8.1 Rules [CS-LOG-01]

- Inject `ILogger<T>` — never use `Console.WriteLine` or concrete logger types outside composition root
- Use structured logging: `_logger.LogInformation("Archived {RecordingId}", id);` — not string interpolation in the template
- Never log passwords, tokens, or PII

### 8.2 Log Levels [CS-LOG-02]

| Level | Use for |
|---|---|
| `Trace` | Very detailed diagnostics, off in production |
| `Debug` | Developer diagnostics, off in production |
| `Information` | High-level progress: service start, significant events |
| `Warning` | Unexpected but recoverable conditions |
| `Error` | Failed operations with user or system impact |
| `Critical` | System unusable; requires immediate attention |

### 8.3 Correlation IDs [CS-LOG-03]

Every request carries a correlation ID propagated across services via the `traceparent` header. Every log entry produced during the request includes the correlation ID via log scopes or enrichers.

---

## 9. Dependency Injection [CS-DI]

We use the built-in `Microsoft.Extensions.DependencyInjection` container. Third-party containers require explicit approval.

- Constructor injection only — no property injection, no service locator
- Lifetimes: `Singleton` for stateless shared services; `Scoped` for per-request state (DbContext); `Transient` for lightweight, stateless services
- Never inject a shorter-lived service into a longer-lived one (scoped into singleton)
- Every public service is registered against its interface, not its concrete type

---

## 10. Security [CS-SEC]

- Never store secrets in code or configuration files checked into source control — use environment variables or a secrets manager
- Validate all input at system boundaries — don't trust data arriving from external sources
- Apply the principle of least privilege to service accounts and API keys
- Parameterize all database queries — never concatenate user input into SQL
- Log security-relevant events: authentication failures, access denials, sensitive data access
- Review security implications for any change touching authentication, authorization, or data handling
