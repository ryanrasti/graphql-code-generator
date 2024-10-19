# `@graphql-codegen/client-preset-swc-plugin`

When using the [`@graphql-codegen/client-preset`](https://the-guild.dev/graphql/codegen/plugins/presets/preset-client) on large scale projects might want to enable code splitting or tree shaking on the `client-preset` generated files. This is because instead of using the map which contains all GraphQL operations in the project, we can use the specific generated document types.

This plugin works for [SWC](https://swc.rs) only.

### Installation

```bash
yarn add -D @graphql-codegen/client-preset-swc-plugin
```

### Usage

You will need to provide the `artifactDirectory` path that should be the same as the one configured in your `codegen.ts`

#### Vite

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react-swc'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    react({
      plugins: [
        ['@graphql-codegen/client-preset-swc-plugin', { artifactDirectory: './src/gql', gqlTagName: 'graphql' }]
      ]
    })
  ]
})
```

#### Next.js

```ts
const nextConfig = {
  // ...
  experimental: {
    swcPlugins: [
      ['@graphql-codegen/client-preset-swc-plugin', { artifactDirectory: './src/gql', gqlTagName: 'graphql' }]
    ]
  }
}
```

#### `.swcrc`

```json5
{
  // ...
  jsc: {
    // ...
    experimental: {
      plugins: [
        ['@graphql-codegen/client-preset-swc-plugin', { artifactDirectory: './src/gql', gqlTagName: 'graphql' }]
      ]
    }
  }
}
```

### Release

To publish a new version ensure you have done the following:

- Update the version in `package.json`
- Update the `CHANGELOG.md` with the new version and changes
- Commit the changes
- From GitHub Actions, run the `Rust plugin` workflow

### Building for different versions of `swc_core`
- Update `swc_core` version in `Cargo.toml` (see https://swc.rs/docs/plugin/selecting-swc-core for how
  to choose the right version to match the npm package version you want to build it for)
- Run `npm run build-wasm`
- Publish with `npm publish --access=public`
