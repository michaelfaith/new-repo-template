# Differences from Base CTA (with Everything)

## Embrace tsdown Default Settings

- `lib` -> `dist` for build output (remove outDir)
  - update all files that reference lib (vitest.config.ts, .gitignore, eslint.config.ts, .prettierignore, cspell.json, package.json, DEVELOPMENT.md)
- bundle entry point (remove `unbundle`)
- when bundle is true, then only the `src/index.ts` needs to be the entry point, and that's already the default if it exists, so `entry` can be removed
- `fixedExtension` -> true (the default)
  - index.js -> index.mjs in ci workflow and `package.json`
- `dts` as true is already the default if types are defined, so this can be removed

## tsconfig

- remove `declarationMap`
- enable `isolatedModules` and `isolatedDeclarations`
  - add function return type to `greet`
- lowercase `nodenext` to match typescript docs
- enable `rewriteRelativeImportExtensions`
  - change all relative imports to use `.ts` extension
- change target to `es2024` (from `es2022`)

## ESLint Config

- explicit type imports and exports (`@typescript-eslint/consistent-type-exports`, `@typescript-eslint/consistent-type-imports`)
  - add `type` to import in `greet.ts` and
- enforce module boundaries (`@typescript-eslint/explicit-module-boundary-types`)
- Switch to `@ianvs/prettier-plugin-sort-imports` instead of perfectionist for import sorting
  - `importOrder: ['<BUILTIN_MODULES>', '', '<THIRD_PARTY_MODULES>', '', '^[.]'],`
  - `importOrderTypeScriptVersion: '6.0.0',`
- only use perfectionist for export sorting (`perfectionist/sort-exports`)
- remove `yml/sort-keys` from config
- add config to apply `yml/sort-keys` to only the pnpm-workspace.yaml
- remove `n/no-unpublished-bin` disable
- enable `vitest/prefer-describe-function-title`
- disable `n/no-unsupported-features/node-builtins` for `eslint.config.ts` and tests

## Workflows

- re-order the properties in the workflow yaml's to be more standard order instead of alphabetical
- re-order the properties in actions and issue_template yaml's to be more standard order
- add empty lines between jobs in workflows
- move all workflows to the built-in GH token instead of using PAT
- change the node version in Prepare action to `lts/*`
- change the name of the "Prepare" action to "Setup"
- release-it -> release please
  - delete post-release workflow and build the release commenting into the release workflow
  - add `autorelease` tag to the octoguide if condition
- add `node-version` matrix to test job in ci
- add ["Engines Check"](https://github.com/michaelfaith/eslint-plugin-package-json/blob/8d847ed2180cbcfc891d7b82be69ec5b7cf7dd3b/.github/workflows/ci.yml#L52-L70) step to the CI workflow

## Prettier Config

- remove `useTabs`
- singleQuote: true

## VSCode Settings

- change `editor.codeActionsOnSave` -> `source.fixAll.eslint` to `always`
- remove `eslint.rules.customizations`
- change `typescript.tsdk` to ` js/ts.tsdk.path` (deprecated)

## Other Code Changes

- `knip.json` -> `knip.config.ts`
- `.prettierrc.json` -> `prettier.config.ts`
- `simple-git-hooks` + `pretty-quick` instead of `lint-staged` + `prettier`
  - remove `prepare: "husky"` from package.json
  - add `.simple-git-hooks.js` to `eslint.config`'s allowDefaultProject
- upgrade `pnpm` to v11
- add `pnpm-workspace.yaml` with `trustPolicy: no-downgrade` and `allowBuilds simple-git-hooks: true`
- increase node version in nvmrc (24.18.0)
- change engines to `^22.11.0 || ^24.11.0 || >=26.0.0`
- update `tsdown` to 0.22.4
- update typescript to 6.0.3
- update `eslint`, `@eslint/js` to v10
- update `@eslint/markdown` to v8
- update `knip` to v6
- install `jiti` - required for `eslint.config.ts` support
- add `"type": "./dist/index.d.mts"` to `exports` in `package.json`
- change exports in `index.ts` to named exports and add `type` to types export
- add Version input to Bug issue template
- remove code of conduct and typescript strict badges from `README.md`
- remove `patch.enable: false` from renovate.json
- add `package.json` and `prettier.config.ts` to cspell's `ignorePaths`

## GitHub Settings

- Disable Projects
- Enable "Limit how many branches and tags can be updated in a single push"

## Additional Info

Here's a PR on a repo created by CTA --everything, that encapsulates all of these changes: https://github.com/michaelfaith/new-repo-template/pull/1
