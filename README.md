<p align="center">
  <img alt="Logo" src="https://i.imgur.com/bUYlxms.png?1" width="420">
</p>

<p align="center">
  <strong>@pika/pack</strong> • A simple, holistic approach to package building & publishing.
</p>

<p align="center">
  <img alt="Logo" src="https://i.imgur.com/T5dGOMa.gif" width="680">
</p>

## What is @pika/pack?

⚡️ @pika/pack is a completely new approach to package building and bundling:

- **Easy to Use:** Compose your build out of pluggable, zero-configuration builders.
  - Say goodbye to complex bundlers and config files!
  - Simple enough for anyone to pick up, but expressive enough to handle the trickiest scenarios.
- **Optimized by Default:** Each builder plugin optimizes your code for one specific environment.
  - Stop publishing bloated, transpiled, Node.js-specific JavaScript to all consumers.
  - Create a Node.js-ready build, a web-optimized ESM build, auto-generate TypeScript definitions...
- **Holistic:** Builder plugins configure your `package.json` entrypoints (like `"main"` &`"module"`) as well.
  - Really, when we say simple, we mean it!
  - All you need to do is include the build plugin, and then it handles the rest.

The result of running @pika/pack is a self-contained, ready-to-run `pkg/` dir. Link it, run it locally, and publish it to npm when you're ready. Your package build directory only includes neccessary files by default, so no more `"files"` or `.npmignore` configuration to worry about either.


## Quickstart

```
npm install --global @pika/pack
```

To use @pika/pack, define a build `"pipeline"` in your source project's `package.json` manifest (similar to the "plugins" section of a `.babelrc` file):

```js
/* Before: Your Project package.json */

{
  "name": "simple-package",
  "version": "1.0.0",
  "@pika/pack": {
    // Define the pipeline that will build your package:
    "pipeline": [
      // 1. Compiles your source to standard ES2018+
      ["@pika/plugin-standard-pkg", {"exclude": ["__tests__/*"]}],
      // 2. Creates a distribution optimized to run on Node.js
      ["@pika/plugin-build-node"],
      // 3. Creates a distribution optimized for web browsers & bundlers
      ["@pika/plugin-build-web"],
      // 4. Automatically generates type definitions from your JavaScript
      ["@pika/plugin-build-types"]
    ]
  },
  // ...
}
```

No other configuration or tooling needed! When you run `pika build` in your project you'll get a built `pkg/` directory, with all `package.json` entrypoints (like `"main"` and `"module"`) added automatically:

```js
/* After: Your generated `pkg/` package.json manifest: */

{
  "name": "simple-package",
  "version": "1.0.0",
  "esnext": "dist-src/index.js",
  "main": "dist-node/index.js",
  "module": "dist-web/index.js",
  "types": "dist-types/index.d.ts",
  "sideEffects": false,
  "files": ["dist-*/", "assets/", "bin/"]
}
```

[See a full collection of example projects here →](https://github.com/pikapkg/examples)


## Available Builders

#### Source Builders:
 - `@pika/plugin-standard-pkg`: Compiles JavaScript/TypeScript to ES2018.
 - `@pika/plugin-ts-standard-pkg`: Compiles TypeScript to ES2018 (Uses `tsc` instead of Babel, and builds type definitions automatically).

#### Distribution Builders:

 - `@pika/plugin-build-deno`: Builds a distribution that runs on Deno (TS projects only).
 - `@pika/plugin-build-node`: Builds a distribution that runs on Node LTS (v6+).
 - `@pika/plugin-build-types`: Builds TypeScript definitions from your TS, or automatically generate them from your JS. Not required if you use `@pika/plugin-ts-standard-pkg`.
 - `@pika/plugin-build-web`: Builds an ESM distribution optimized for browsers & bundlers.

#### WASM Builders:
 - `@pika/plugin-wasm-assemblyscript`: Builds WASM from TypeScript using [AssemblyScript](https://github.com/AssemblyScript/assemblyscript).
 - `@pika/plugin-wasm-bucklescript`: Builds WASM from ReasonML/OCAML using [BuckleScript](https://bucklescript.github.io/).
 - `@pika/plugin-wasm-bindings`: Builds a simple JS wrapper for any WASM build.

#### Advanced Builders:
 - `@pika/plugin-bundle-node`: Creates a Node.js build with all code (including dependencies) bundled into a single file. Useful for CLIs.
 - `@pika/plugin-bundle-web`: Creates a ESM build with all code (including dependencies) bundled. Useful for unpkg & serving code directly to browsers.
 - `@pika/plugin-simple-bin`:  Generates & configures a CLI wrapper to run your library from the command line.
- **Write your own!** @pika/pack can load local builders by relative path directly from your repo.
- **Publish & Share your own!** These official builders are just the start. Create a PR to add your community plugin to this list.

[See a full list of official builders here →](https://github.com/pikapkg/builders/tree/master/packages)


## Commands

### pika build

![Imgur](https://i.imgur.com/rSY77ks.gif)


### pika publish

Based on the popular [`np`](https://github.com/sindresorhus/np) package! Validates your directory before running you through the publish process step-by-step to publish your `pkg/` sub-directory package.
![publish screenshot](https://imgur.com/SPjSRGN.png)
