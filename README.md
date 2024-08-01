# Description

This repository showcases a working example of lerna + nx caching.

## Pre-requisites

Successfully tested with the following.

- OS: Windows 10
- Node: v18.15.0
- Yarn: 1.22.19
- Lerna: 6.6.2

## Reproduction steps

- Run `yarn`
  - For repeated attempts - run `npx nx reset` to clear existing cache
- Run `yarn build`

  - Get output like this, cache not used:

  ```
  yarn run v1.22.19
  $ lerna run build --scope='example-*'
  lerna notice cli v6.6.2
  lerna info versioning independent
  lerna notice filter including "example-*"
  lerna info filter [ 'example-*' ]

      √  example-lib-2:build (2s)
      √  example-lib-1:build (2s)

   >  Lerna (powered by Nx)   Successfully ran target build for 2 projects (3s)

  Done in 5.46s.
  ```

- Run `yarn build` again:

  - Get output like this, cache used for both libs:

  ```
  yarn run v1.22.19
  $ lerna run build --scope='example-*'
  lerna notice cli v6.6.2
  lerna info versioning independent
  lerna notice filter including "example-*"
  lerna info filter [ 'example-*' ]

      √  example-lib-2:build  [existing outputs match the cache, left as is]
      √  example-lib-1:build  [existing outputs match the cache, left as is]


   >  Lerna (powered by Nx)   Successfully ran target build for 2 projects     (61ms)

     Nx read the output from the cache instead of running the command for 2 out    of 2 tasks.

  Done in 2.25s.
  ```

- Adjust the `index.ts` in one of the libs, e.g. `"Hello World 2!"` -> `"Hello World 3!"`
- Run `yarn build` again:

  - Get output like this, cache is used only for unchanged lib:

  ```
  yarn run v1.22.19
  $ lerna run build --scope='example-*'
  lerna notice cli v6.6.2
  lerna info versioning independent
  lerna notice filter including "example-*"
  lerna info filter [ 'example-*' ]

      √  example-lib-1:build  [existing outputs match the cache, left as is]
      √  example-lib-2:build (2s)

   >  Lerna (powered by Nx)   Successfully ran target build for 2 projects (2s)

     Nx read the output from the cache instead of running the command for 1 out of 2 tasks.

  Done in 4.60s.
  ```
