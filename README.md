# Reproduction: @cloudflare/playwright crashes when imported outside Workers

The top-level `import { env } from 'cloudflare:workers'` in `@cloudflare/playwright` causes the module to crash when loaded in non-Worker runtimes (Node.js, Deno, Bun).

## Steps to reproduce

```bash
pnpm install
node index.mjs
```

## Expected

```
Module loaded successfully
chromium: object
```

## Actual

```
node:internal/modules/esm/hooks:835
  const url = getValidatedURL(urlString);
              ^
TypeError [ERR_INVALID_URL]: Invalid URL
    at new URL (node:internal/url:385:13)
    ...
```

Or in some Node.js versions:

```
Error: Only URLs with a scheme in: file, data, and node are supported by the default ESM loader. Received protocol 'cloudflare:'
```

## Environment

- Node.js 20+
- `@cloudflare/playwright@1.3.0`

## Related

- Draft PR: https://github.com/cloudflare/playwright/pull/193
- Related issue: https://github.com/cloudflare/playwright/issues/88
- Related PR: https://github.com/cloudflare/playwright/pull/59
