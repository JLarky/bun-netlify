# React with Bun runtime

This is a React project bootstrapped with [bun](https://bun.sh/).

## Getting Started

### Cloning the repo

```sh
bun create react ./react-bun-app
```

### Development

First, run the development server.

```
bun dev
```

Open http://localhost:3000 with your browser to see the result.

You can start editing the page by modifying src/App.jsx. The page auto-updates as you edit the file.

## Prepare for builds

[For react-scripts checkout main branch](https://github.com/JLarky/bun-netlify/tree/main)

Add `vite` and `@vitejs/plugin-react` to `devDependencies`:

```
bun add -d vite @vitejs/plugin-react
```

Configure vite in `vite.config.ts`:

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [
    react()
  ]
})
```

## Connect to netlify

make sure to connect your repo to github and netlify to enable automatic builds.

```
ntl init
```

Edit `netlify.toml`:

```toml
[build]
  # custom build command that install bun and run build
  command = "./scripts/build.sh"
  # vite output folder
  publish = "dist"

[build.environment]
   # disable NPM install
   NPM_FLAGS = "--version"
```

Edit `scripts/build.sh`:

```sh
#!/bin/bash
set -e
curl -fsSL https://bun.sh/install | bash
export PATH="/opt/buildhome/.bun/bin:$PATH"
bun --version
bun install
bun vite build
```

## Deploy

Just push to github and netlify will build and deploy your site.

## Learn More

- [How to build production build of Bun React](https://dev.to/ashirbadgudu/create-a-react-app-with-bun-125o)
- [How to install Bun](https://bun.sh/)
- [Install Deno on Netlify](https://dbushell.com/2021/07/22/netlify-deno-builds/) --- we use the same idea to install Bun
- [How to disable NPM install](https://answers.netlify.com/t/prevent-npm-from-running-on-deploy/66882/3)
