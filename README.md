# @tescord/type-gen

A TypeScript type generation module for tescord localization. This module analyzes tescord instances and generates TypeScript declarations for type-safe localization usage.

## Features

- 🔍 **Smart Parameter Detection**: Automatically detects parameters in localization strings
- 🏷️ **Named Parameters**: Supports both indexed (`{0}`, `{1}`) and named (`{0:user}`, `{1:role}`) parameters
- 🔗 **Multi-Client Support**: Generate types for multiple tescord instances
- 📁 **Nested Structures**: Handles nested localization objects
- 💾 **File Output**: Write generated types directly to TypeScript declaration files

## Installation

```bash
npm install @tescord/type-gen
# or
pnpm add @tescord/type-gen
# or
yarn add @tescord/type-gen
```

## Usage

### Basic Usage

```typescript
import { generateLocalizationTypes, createTypeGenerationConfig } from '@tescord/type-gen';
import { tescord } from 'tescord';

// Create your tescord instance
const tescord = new tescord({
  id: 'my-bot',
  clients: [{
    id: 'main-client',
    token: 'your-bot-token',
    options: { intents: [] }
  }]
});

// Generate types
const config = createTypeGenerationConfig(tescord);
const types = generateLocalizationTypes(config);

console.log(types);
```

### Multiple Clients

```typescript
import { generateLocalizationTypes, createTypeGenerationConfig } from '@tescord/type-gen';

const config = createTypeGenerationConfig([tescord1, tescord2, tescord3]);
const types = generateLocalizationTypes(config);
```

### Write to File

```typescript
import { writeLocalizationTypes } from '@tescord/type-gen';

// Option 1: Use default output path (process.cwd()/localization.d.ts)
const config = createTypeGenerationConfig(tescord);
await writeLocalizationTypes(config);

// Option 2: Specify custom output path in config
const configWithPath = createTypeGenerationConfig(tescord, './types/localization.d.ts');
await writeLocalizationTypes(configWithPath);
```

## Parameter Detection Examples

The module automatically detects and converts localization parameters:

### Indexed Parameters
```
Input:  "{0} is now a {1}"
Output: (_0: string, _1: string) => string
```

### Named Parameters
```
Input:  "{0:user} is now a {1:role}"
Output: (user: string, role: string) => string
```

### No Parameters
```
Input:  "Hello, world!"
Output: () => string
```

## Generated Output Example

For a tescord instance with localization data, the module generates:

```typescript
declare global {
    namespace tescord {
        interface Localization {
            hello: () => string;
            welcome: (username: string) => string;
            userJoined: (user: string, server: string) => string;
            nested: {
                commands: {
                    help: () => string;
                    ping: (target: string) => string;
                };
                messages: {
                    error: (errorMessage: string) => string;
                    success: () => string;
                };
            };
        }
    }
}
```

## Multi-Client Output

When multiple tescord instances are provided:

```typescript
declare global { // Client 1
    namespace tescord {
        interface Localization {
            hello: () => string;
            world: (exampleParam: string) => string;
        }
    }
}

declare global { // Client 2
    namespace tescord {
        interface Localization {
            secondClientHello: () => string;
        }
    }
}
```

## API Reference

### `generateLocalizationTypes(config: TypeGenerationConfig): string`

Generates TypeScript declaration strings from tescord instances.

**Parameters:**
- `config`: Configuration object containing tescord instances

**Returns:** Generated TypeScript declaration string

### `createTypeGenerationConfig(tescords: tescord | tescord[], outputPath?: string): TypeGenerationConfig`

Creates a configuration object for type generation.

**Parameters:**
- `tescords`: Single tescord instance or array of instances
- `outputPath`: Optional custom output path (defaults to `process.cwd()/localization.d.ts`)

**Returns:** Configuration object

### `writeLocalizationTypes(config: TypeGenerationConfig): Promise<void>`

Writes generated types to a file using the output path from the configuration.

**Parameters:**
- `config`: Configuration object (containing tescords and optional outputPath)

## Types

### `TypeGenerationConfig`

```typescript
interface TypeGenerationConfig {
  tescords: tescord[];
  outputPath?: string;
}
```

### `ExtractedParameter`

```typescript
interface ExtractedParameter {
  name: string;
  type: string;
}
```

## Contributing

This module is part of the tescord ecosystem and is currently in development. It is not yet ready for production use.

## License

LGPL-3.0-or-later

## Authors

- Kıraç Armağan Önal
- Erdem Göksel
