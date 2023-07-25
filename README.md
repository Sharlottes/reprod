# vanilla-extract #1101 issue reproduction

## Test Environment

|system|version|
| -- | -- |
| node | v18.13.0 |
| package-management | yarn@v1.22.19 |
| os | windows 10 |

## how to start

0. install packages
```bash
yarn
```

2. run `next dev`

```bash
yarn dev
```

2. below error log occured and process died.

```
$ yarn dev
yarn run v1.22.19
$ next dev
- warn Port 3000 is in use, trying 3001 instead.
- ready started server on 0.0.0.0:3001, url: http://localhost:3001
TypeError: Cannot read properties of undefined (reading 'loader')
    at getVanillaExtractCssLoaders (C:\Users\user\Documents\GitHub\reprod\node_modules\@vanilla-extract\next-plugin\dist\vanilla-extract-next-plugin.cjs.dev.js:41:40)
    at Object.webpack (C:\Users\user\Documents\GitHub\reprod\node_modules\@vanilla-extract\next-plugin\dist\vanilla-extract-next-plugin.cjs.dev.js:97:12)
    at getBaseWebpackConfig (C:\Users\user\Documents\GitHub\reprod\node_modules\next\dist\build\webpack-config.js:2147:32)
    at async Promise.all (index 0)
    at async Span.traceAsyncFn (C:\Users\user\Documents\GitHub\reprod\node_modules\next\dist\trace\trace.js:103:20)
    at async Span.traceAsyncFn (C:\Users\user\Documents\GitHub\reprod\node_modules\next\dist\trace\trace.js:103:20)
    at async HotReloader.start (C:\Users\user\Documents\GitHub\reprod\node_modules\next\dist\server\dev\hot-reloader.js:573:30)
    at async DevServer.prepareImpl (C:\Users\user\Documents\GitHub\reprod\node_modules\next\dist\server\dev\next-dev-server.js:685:9)
    at async NextServer.prepare (C:\Users\user\Documents\GitHub\reprod\node_modules\next\dist\server\next.js:165:13)
    at async Server.<anonymous> (C:\Users\user\Documents\GitHub\reprod\node_modules\next\dist\server\lib\render-server.js:136:17) {
  type: 'TypeError'
}
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
``
```

## how to create this repo

1. create next app.

```bash
yarn create next-app
yarn add @vanilla-extract/css @vanilla-extract/next-plugin
```

CNA setting is like below log.

```bash
      - create-next-app
√ What is your project named? ... reprod
√ Would you like to use TypeScript? ... No / Yes
√ Would you like to use ESLint? ... No / Yes
√ Would you like to use Tailwind CSS? ... No / Yes
√ Would you like to use `src/` directory? ... No / Yes
√ Would you like to use App Router? (recommended) ... No / Yes
√ Would you like to customize the default import alias? ... No / Yes
Creating a new Next.js app in C:\Users\user\Documents\Github\reprod.

```

2. change `next.config.js` to below code

```
const { createVanillaExtractPlugin } = require("@vanilla-extract/next-plugin");
const withVanillaExtract = createVanillaExtractPlugin();

/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    appDir: true,
  },
  reactStrictMode: true,
};

module.exports = withVanillaExtract(nextConfig);
```

not sure `experimental.appDir` do something. but although without appDir, the result was same.
