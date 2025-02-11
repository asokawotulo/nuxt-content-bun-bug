This repository is the reproduction of a bug I encounter while using `@nuxt/context` with `bun`. 

Steps to reproduce:
1. `bun install`
2. `bun build`
3. `bun run .output/server/index.mjs`
4. Open `http://localhost:3000` in your browser
5. Check the console, you should see that the process crashed with the following error: 
   
```bash
bun: symbol lookup error: /home/melkam/nuxt-content-bug/.output/server/node_modules/better-sqlite3/build/Release/better_sqlite3.node: undefined symbol: _ZN2v816FunctionTemplate16InstanceTemplateEv
```

If you try to run it with `node`, it works fine. I'm not sure if this is a bug in `bun` or `@nuxt/content`. 
