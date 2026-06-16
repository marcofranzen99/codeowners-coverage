# Contributing

## Prerequisites

- [Node.js](https://nodejs.org/) (v20 or later)
- [pnpm](https://pnpm.io/)

### Install pnpm

```sh
npm install -g pnpm
```

## Setup

Install dependencies:

```sh
pnpm install
```

## Build

Bundle the action into `dist/index.js`:

```sh
pnpm build
```

This runs `ncc build src/index.ts` which compiles and bundles all dependencies into a single file.

## Test

```sh
pnpm test
```

## Lint

```sh
pnpm lint
```
