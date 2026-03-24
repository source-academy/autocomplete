# Autocomplete Plugin

## Using the package

For most circumstances, using the package directly would be fine

```bash
yarn add @sourceacademy/autocomplete-plugin@github:source-academy/autocomplete-plugin#0.0.1
```

Then, directly import them from your code

```ts
import { BaseAutoCompleteWebPlugin } from '@sourceacademy/autocomplete-plugin';

export class AutoCompletePlugin extends BaseAutoCompleteWebPlugin {
    (...)
}
```

If you need to set up a development build, clone the repository

```bash
git clone https://github.com/source-academy/autocomplete-plugin
cd autocomplete-plugin
```

Run `yarn build` (or `yarn watch` for development, where it creates incremental builds). If there are no problems, the files `dist/index.mjs` and `dist/index.cjs` will be generated.
This is the file that will be used to run the plugin on the host (the frontend) and the runner.

To use the new package in another project, go to the project directory, then use

```bash
yarn link path/to/autocomplete-plugin
```

## Contents of this package

The project uses Yarn v4 with [PnP](https://yarnpkg.com/features/pnp) out of the box.
**To generate the Typescript SDK if you're using VSCode, run `yarn dlx @yarnpkg/sdks vscode`. Otherwise, types won't show up properly**. For other editors, refer to the [Yarn documentation](https://yarnpkg.com/getting-started/editor-sdks).

It is already configured with [ESLint](https://eslint.org/) and [Prettier](https://prettier.io/). Linting is available through `yarn lint` and formatting through `yarn format`. **Linting and formatting changes are enforced in CI builds, in line with other SA repos**. A minimal testing framework is provided with `jest`. Testing is performed using `yarn test`.

_Note: For more information regarding the terminology and systems used in this package, refer to the [Conductor](https://github.com/source-academy/conductor) repository. It'll make the paragraphs below a lot more coherent_

The repo consists of two abstract plugins, `BaseAutoCompleteWebPlugin` in `src/web.ts` which is meant to be run on the host's side (the frontend, most likely), as well as the `BaseAutoCompleteRunnerPlugin` in `src/runner.ts` meant to be run on the runner's side (the thread running the evaluator).

The plugins subscribe to two channels

1. An autocomplete channel. The usual flow is that every time an edit is made in the frontend, the web plugin sends an `AutoCompleteRequest` to the runner plugin via `webplugin.autocomplete(...)`. The runner plugin receives it and sends an `AutoCompleteResponse` back.
2. A syntax highlighting channel. The runner plugin periodically sends syntax highlighting information using `SyntaxHighlightMessage` till it receives an acknowledgement `SyntaxHighlightAck`. Alternatively, the web plugin can send a `SyntaxHighlightRequest` instead of waiting for the periodic messages.

The Typescript files are bundled using [Rollup](https://rollupjs.org/), and are transpiled into the `dist` folder (at `dist/index.mjs` and `dist/index.cjs`, as well as the type definitions at `dist/index.d.ts`).

## Documentation

This repository has been configured to automatically build documentation and deploy it to GitHub Pages using Typedoc upon pushing to the `main` branch on GitHub.
