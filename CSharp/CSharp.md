# C# Coding Style

## Priority

- When editing an existing file, follow that file's existing style; the file wins over this document
- Exception: when the task itself is a style refactor, this document is the target style instead of the old style
- Use these rules when the file has no established style; use widely accepted practice for brand-new components
- Style and naming changes ship as standalone commits, never mixed with functional changes

## Formatting

- Allman braces: opening brace on its own line
- Wrap every statement block in braces, including single-line `if` / `for` / `foreach`
- 4-space indentation, no tabs
- One statement per line, one declaration per line
- One blank line between method and property definitions; no consecutive blank lines elsewhere, no stray spaces inside lines
- Declare namespaces in file-scoped form: `namespace Foo;`
- Place `using` directives outside the `namespace` declaration, sorted alphabetically with `System.*` first

```csharp
if (condition)
{
    DoSomething();
}
```

## Naming

- Private fields use `lowerCamelCase` without a `_` prefix
- Add `readonly` whenever possible; if a field must be reassigned after construction, keep it a plain field rather than turning it into a property
- Constant fields and constant locals use `PascalCasing`; interop constants must match the name and value of the native code exactly

## Types and Members

- Call static members through the type name: `ClassName.StaticMember`, never through a derived type name
- Qualify a field with `this.` when a parameter or local shadows its name
- Use language keywords for both type references and method calls instead of BCL type names: `int` / `string` / `float`, `int.Parse` rather than `Int32.Parse`
- Use `var` only when the type is obvious from the right-hand side, typically a literal, `new`, or an explicit cast; a `foreach` element type is not obvious
- Use `var` for LINQ query variables and result sequences

```csharp
// OK: new makes the type obvious
var stream = new FileStream(path, FileMode.Open);

// Not OK: the return type is not obvious
var stream = OpenStandardInput();

// Correct: language keyword
int count = int.Parse(text);

// Forbidden: BCL type name
Int32 count = Int32.Parse(text);
```

## Syntax and Language Features

- Prefer the current C# version's new syntax; avoid obsolete language constructs
- Initialize collections with collection expressions: `string[] vowels = ["a", "e", "i", "o", "u"];`
- Use target-typed `new()` when the variable type matches the created type: `ExampleClass item = new();`
- Create objects with object initializers instead of assigning properties line by line
- Mark required properties with `required` instead of forcing initialization through constructors
- Use `Func<>` / `Action<>` for internal callbacks and one-off functions; do not define new delegate types
- For public APIs, events, or functions with real business meaning, use a named delegate with XML documentation
- Define event handlers with lambdas when they do not need to be removed later
- Release resources with a `using` declaration or `using` statement instead of hand-written try-finally calling `Dispose`
- Primary constructor parameters: PascalCasing on records, camelCase on classes and structs
- Use `&&` / `||` in conditions instead of `&` / `|` to get short-circuit evaluation; bitwise operations still use `&` / `|`

## Exceptions

- Catch only exceptions you can actually handle; never catch `System.Exception` without a filter
- Catch specific exception types and provide meaningful messages

## Async

- Use `async` / `await` for I/O-bound work; never block synchronously
- In library code with a synchronization context (UI, legacy ASP.NET), use `ConfigureAwait(false)`; ASP.NET Core and console apps do not need it

## Strings

- Concatenate short strings with interpolation, in the expression-based form: `$"{student.Last} Score: {student.score}"`
- Use `StringBuilder` for heavy appends inside loops
- Prefer raw string literals `"""..."""` when content contains escapes or spans multiple lines

## Comments

- Use single-line `//` comments for brief explanations; do not use `/* */` for long explanations
- Put comments on their own line, never at the end of a code line
- Start comment text with a capital letter, end with a period, and put one space after `//`
- Use XML comments for public classes, methods, and fields

## Editing Existing Code

- Change only the code the task touches; do not reformat adjacent code, except during a style refactor
- During a style refactor, unify the target scope to this document, keep behavior unchanged, and ship it separately from functional changes
- Do not rename members bound by external contracts: serialization, `DataContract`, reflection, ORM mappings, data binding, P/Invoke structs, published public APIs
- When an existing file uses `_`-prefixed fields and the change is small, name new fields `_lowerCamelCase` to stay consistent with the file
