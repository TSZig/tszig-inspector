# tszig-inspector

Package analyzer and TypeScript stub generator for Zig libraries.

## Overview

`tszig-inspector` analyzes Zig packages and generates TypeScript declaration files (`.d.ts`) that enable IDE support and type checking when using those packages with TSZig.

## Features

- **Package Analysis**: Inspect Zig packages to extract public API information
- **Stub Generation**: Generate TypeScript declaration files from Zig code
- **Type Mapping**: Automatically map Zig types to TypeScript equivalents
- **IDE Support**: Enable IntelliSense for Zig libraries in TypeScript editors

## Installation

### From Source

```bash
git clone https://github.com/TSZig/tszig-inspector
cd tszig-inspector
zig build
```

## Usage

### Analyze a Zig Package

```bash
tszig-inspector analyze std
```

Output:
```
Package: std
Modules: 47
Public Functions: 523
Public Types: 189
```

### Generate TypeScript Stubs

```bash
tszig-inspector stub std > std.d.ts
```

### Generate Type Definitions

```bash
tszig-inspector types ./my-lib > my-lib.d.ts
```

## Example Output

Input Zig code:
```zig
pub fn readFile(path: []const u8) ![]u8 {
    // ...
}

pub const FileError = error{
    NotFound,
    AccessDenied,
    IoError,
};
```

Generated TypeScript stub:
```typescript
declare module "my-lib" {
    export function readFile(path: string): string;
    
    export type FileError = 
        | "NotFound"
        | "AccessDenied"
        | "IoError";
}
```

## Type Mapping

| Zig Type | TypeScript Type |
|----------|-----------------|
| `i32`, `i64`, `f32`, `f64` | `number` |
| `[]const u8` | `string` |
| `bool` | `boolean` |
| `void` | `void` |
| `?T` | `T \| null` |
| `!T` | `T` (throws on error) |
| `[]T` | `T[]` |
| `struct { ... }` | `interface { ... }` |
| `enum { ... }` | `type = ...` |

## CLI Reference

```bash
tszig-inspector <command> [options] <target>

Commands:
  analyze <package>    Analyze package and show summary
  stub <package>       Generate TypeScript stub declarations
  types <package>      Generate .d.ts type definitions
  
Options:
  --output, -o <file>  Write output to file
  --verbose, -v        Show detailed output
  --help, -h           Show help
```

## License

MIT License - see [LICENSE](LICENSE) for details.

## Contributing

See [CONTRIBUTING.md](https://github.com/TSZig/.github/blob/main/CONTRIBUTING.md) for guidelines.
