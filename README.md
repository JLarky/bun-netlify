# How to quickly deploy Bun React project to Netlify

This is a template showing how you can use Bun to build React project and deploy it to Netlify. We are using Vite and React, if you want to change this for your project, please do.

# Important clarifications

A few notes on bun vs bun :)

When you run `bun dev` it will use `bun` just as a script runner, and your vite will actually be running in Node. But if you run `bun --bun dev` if will use Bun JavaScript runtime to run Vite.

If this all sounds crazy, just do this (after following the guide below)):

```bash
echo 'console.log(Bun);' >> vite.config.ts
bun --bun dev # to test it
bun dev # to see that it crashes
```

Again, this doesn't actually matter that much, since it only afffects vite plugins and such, your users are just going to get the same application bundle no matter what runtime vite was using :)

# Requirements

- [Bun 1.0 or higher](https://bun.sh/)

# What to do

You can clone this repo or just follow the steps below.

## 1. Create new React project with Vite using Bun

```bash
bunx --bun create-vite -t react-ts
cd vite-project
bun install
```

## 2. Development

```
bun --bun dev
```

open http://localhost:5173/

## 3. Build

```
bun --bun run build
```

<details>
<summary>
Is it fast? Well, almost as fast as Node https://twitter.com/jarredsumner/status/1700680788231311789 :)
</summary>

Keep in mind that `bun run build` or even `npm run build` will probably run slightly faster. Let me say it again, currently building Vite using Node runtime is faster (shocking, I know).

```
$ hyperfine --warmup=2 "bun --bun run build" "bun run build" "npm run build"

Benchmark 1: bun --bun run build
  Time (mean ± σ):      2.052 s ±  0.012 s    [User: 2.098 s, System: 0.176 s]
  Range (min … max):    2.037 s …  2.072 s    10 runs
 
Benchmark 2: bun run build
  Time (mean ± σ):      1.363 s ±  0.021 s    [User: 1.210 s, System: 0.090 s]
  Range (min … max):    1.344 s …  1.410 s    10 runs
 
Benchmark 3: npm run build
  Time (mean ± σ):      1.599 s ±  0.029 s    [User: 2.527 s, System: 0.192 s]
  Range (min … max):    1.570 s …  1.666 s    10 runs
 
Summary
  'bun run build' ran
    1.17 ± 0.03 times faster than 'npm run build'
    1.51 ± 0.03 times faster than 'bun --bun run build'
```
</details>

## 4. Connect to Netlify

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
bun --bun run build
```

## 5. Deploy

Just push to github and netlify will build and deploy your site.

## Learn More

- [How to build production build of Bun React](https://dev.to/ashirbadgudu/create-a-react-app-with-bun-125o)
- [How to install Bun](https://bun.sh/)
- [Install Deno on Netlify](https://dbushell.com/2021/07/22/netlify-deno-builds/) --- we use the same idea to install Bun
- [How to disable NPM install](https://answers.netlify.com/t/prevent-npm-from-running-on-deploy/66882/3)
- [How to use Bun with Netlify](https://stackoverflow.com/a/74883541)

## Note on `bun dev`

This repo used to use `bun dev` which was deprecated in Bun 1.0 but you can still see those branches:

- [Using "bun dev" and react-scripts for prod](https://github.com/JLarky/bun-netlify/tree/react-scripts)
- [Using "bun dev" and vite for prod](https://github.com/JLarky/bun-netlify/tree/vite)
