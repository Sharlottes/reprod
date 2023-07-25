# vanilla-extract #1101 issue reproduction

## Test Environments

| system             | version       |
| ------------------ | ------------- |
| node               | v18.13.0      |
| package-management | yarn@v1.22.19 |
| os                 | windows 10    |

---

| system             | version       |
| ------------------ | ------------- |
| node               | v18.15.0      |
| package-management | yarn@v1.22.19 |
| os                 | windows 11    |

## how to start

0. install packages

```bash
yarn
```

1. run `next dev`

```bash
yarn dev
```

2. below error log occured and process died.

```bash
$ yarn && yarn dev
yarn install v1.22.19
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
warning "@vanilla-extract/next-plugin > @vanilla-extract/webpack-plugin@2.2.0" has unmet peer dependency "webpack@^4.30.0 || ^5.20.2".
[4/4] Building fresh packages...
Done in 22.39s.
yarn run v1.22.19
$ next dev
- ready started server on 0.0.0.0:3000, url: http://localhost:3000
TypeError: Right-hand side of 'instanceof' is not an object
    at C:\Users\User\Documents\Repositories\reprod\node_modules\@vanilla-extract\next-plugin\dist\vanilla-extract-next-plugin.cjs.dev.js:109:47
    at Array.some (<anonymous>)
    at Object.webpack (C:\Users\User\Documents\Repositories\reprod\node_modules\@vanilla-extract\next-plugin\dist\vanilla-extract-next-plugin.cjs.dev.js:109:25)
    at getBaseWebpackConfig (C:\Users\User\Documents\Repositories\reprod\node_modules\next\dist\build\webpack-config.js:2147:32)
    at async Promise.all (index 2)
    at async Span.traceAsyncFn (C:\Users\User\Documents\Repositories\reprod\node_modules\next\dist\trace\trace.js:103:20)
    at async Span.traceAsyncFn (C:\Users\User\Documents\Repositories\reprod\node_modules\next\dist\trace\trace.js:103:20)
    at async HotReloader.start (C:\Users\User\Documents\Repositories\reprod\node_modules\next\dist\server\dev\hot-reloader.js:573:30)
    at async DevServer.prepareImpl (C:\Users\User\Documents\Repositories\reprod\node_modules\next\dist\server\dev\next-dev-server.js:685:9)
    at async NextServer.prepare (C:\Users\User\Documents\Repositories\reprod\node_modules\next\dist\server\next.js:165:13)
    at async Server.<anonymous> (C:\Users\User\Documents\Repositories\reprod\node_modules\next\dist\server\lib\render-server.js:136:17) {
  type: 'TypeError'
}
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
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

```ts
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
