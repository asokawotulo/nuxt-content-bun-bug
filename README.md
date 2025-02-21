This repository is the reproduction of a bug I encounter while using `@nuxt/context` with `bun`. 

Steps to reproduce:
1. `bun install`
2. `bun run --bun build`
3. `bun run --bun ./.output/server/index.mjs`
4. Open `http://localhost:3000` in your browser
5. Check the console, you should see that the process crashed with the following error: 
   
```bash
Database integrity check failed 4526 | });
4527 | 
4528 | async function decompressSQLDump(base64Str, compressionType = "gzip") {
4529 |   const binaryData = Uint8Array.from(atob(base64Str), (c) => c.charCodeAt(0));
4530 |   const response = new Response(new Blob([binaryData]));
4531 |   const decompressedStream = response.body?.pipeThrough(new DecompressionStream(compressionType));
                                                                   ^
ReferenceError: Can't find variable: DecompressionStream
      at <anonymous> (/home/melkam/nuxt-content-bug/.output/server/index.mjs:4531:61)
      at decompressSQLDump (/home/melkam/nuxt-content-bug/.output/server/index.mjs:4528:34)

[nuxt] [request error] [unhandled] [500] no such table: _content_content
```

For some reason [bunsqlite polyfills](https://github.com/nuxt/content/blob/cb105a4eb5e22f4c5b48220c526aa2be7c3fcf71/src/runtime/internal/connectors/bunsqlite.ts) are not being loaded correctly.

I also noticed that if you roll back to `@nuxt/content@3.0.1` the error goes away. So it seems to be a regression that happened after `@nuxt/content@3.1.0`.

This issue should be fixed in `@nuxt/content@3.2.0` but it's still happening.