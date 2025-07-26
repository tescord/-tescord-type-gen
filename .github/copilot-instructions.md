This is a module which is in development. It is not yet ready for production use.

This is a sub module for tescord, which is a TypeScript library for generating types for localization of tescord client.

Localization generation should be calculated from tescord["cache"]["locales"]

for example a string like: "{0} is now a {1}" should be converted to a function like:

```typescript
hello: (_0: string, _1: string) => string;
```

but if it is like this:
"{0:user} is now a {1:role}"

it should be converted to a function like:

```typescript
hello: (user: string, role: string) => string;
```

### Output of the module
Should be a function that will take multiple tescords and use their all locales to merge all data into 1 interface. If there are multiple clients, for each client there would be different declare global statement.

### Example Generated String
```ts
// for first client
declare global {
    namespace tescord {
        interface Localization {
            hello: () => string;
            world: (exampleParam: string) => string;
            nested: {
                welcome: (username: string, serverName: string) => string;
                goodbye: () => string;
            };
            commands: {
                help: () => string;
                ping: () => string;
            };
            messages: {
                error: (errorMessage: string) => string;
                success: () => string;
            };
        }
    }
}

// for second client
declare global {
    namespace tescord {
        interface Localization {
            secondClientHello: () => string;
        }
    }
}

export {};
```

### TODO
tescord has support for component type generation, so we should also generate types for components.
add function writeComponentTypes.

tescord#cache has a property called `interactions` which is a Collection<string, CacheData<Interaction>>()

export type CacheData<T> = {
  path: string[];
  data: T;
}

example output:

```ts
declare global {
  namespace tescord {
    interface ComponentMap {
      'my-button': 'Button';
      'my-select': 'StringSelectMenu';
      'my-modal': 'Modal';
    }
  }
}

export {};
```