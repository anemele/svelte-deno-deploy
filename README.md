# sv

test deploying svelte app to deno.com/deploy

ref: <https://github.com/pluvial/svelte-adapter-deno?tab=readme-ov-file#usage>

run commands:

```sh
npx sv create .
deno add npm:svelte-adapter-deno --dev
```

replace `import adapter from '@sveltejs/adapter-auto';` with
`import adapter from 'svelte-adapter-deno';` in `svelte.config.js`.

then run commands:

```sh
deno run build
deployctl deploy --entrypoint=./build/index.js
```

additional commands:

```sh
# install deployctl
deno install -gArf jsr:@deno/deployctl
```
