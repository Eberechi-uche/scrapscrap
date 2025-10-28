# scrapscrap

Rust-powered Open Graph scraper for Node.js with a TypeScript client helper that works across environments.

## Highlights

- Native Rust scraper compiled with N-API for fast metadata extraction.
- Ergonomic TypeScript API (`scrap(url)`) that returns normalized Open Graph data.
- Optional resource helper (`createOpenGraphResource`) for framework-agnostic state handling.
- Ships ESM output, declaration files, and ready-to-run tests.

## Installation

```bash
npm install scrapscrap
```

The package requires Node.js ≥ 18.17 and a Rust toolchain (Cargo) when building from source.

## Usage

### Basic scraping

```ts
import scrap from "scrapscrap";

const metadata = await scrap("https://example.com/article");

console.log(metadata.title);
console.log(metadata.image);
```

`scrap(url)` resolves with:

```ts
type OpenGraphData = {
  title?: string;
  description?: string;
  image?: string;
  url?: string;
  type?: string;
  siteName?: string;
  raw: Array<{ property: string; content: string }>;
};
```

### Framework-agnostic resource

```ts
import { createOpenGraphResource } from "scrapscrap/client";

const resource = createOpenGraphResource();

resource.subscribe((state) => {
  console.log(state.loading, state.error, state.data);
});

await resource.load("https://example.com/article");
await resource.refresh();
resource.clear();
```

In browser environments, pass a custom `fetcher` option that proxies to a server endpoint running the native scraper.

## Development flow

```bash
# Install dependencies
npm install

# Build native addon + TypeScript output
npm run build

# Run tests (spins up a local HTTP fixture)
npm test
```

The build step uses `@napi-rs/cli` to compile the Rust crate in `native/`. When running for the first time, ensure Cargo can download crates from crates.io.

## Publishing checklist

- [x] `npm run lint`
- [x] `npm test`
- [x] `npm run build`
- [x] Update `CHANGELOG.md` and bump version via `npm version`
- [x] `npm publish --access public`

## License

ISC © Eberechi Uche
# scrapscrap
