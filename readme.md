# ts-node performance

This repository is a show case for https://github.com/TypeStrong/ts-node/issues/754

Run `npm run test` to run tests using `ts-node`. Run `npm run test-js` to run tests without `ts-node`. It compiles TS files using TypeScript compiler and then it runs `mocha` tests for these JS files. Surprisingly, JS-based tests are faster than tests using `ts-node` (unless cache is used). If I set `files: true` in `init.js`, the performance is better. 

## How did I measure things

I updated the dist of `ts-node` in `node_modules`, file `index.js`, line `270`:

```javascript
const startTime = Date.now();
var output = service_1.getEmitOutput(fileName);
console.log(`>>> time: ${Date.now() - startTime} ms fileName: ${fileName}`);
``` 

With `files: false`, the compilation of a random `*.spec.ts` takes about 20 ms. With `files: false` it takes about 2 ms. Typical output when I run `npm run test`:

```
>>> time: 452 ms fileName: /ts-node-perf/src/clean-math-1/clean-math.spec.ts
>>> time: 10 ms fileName: /ts-node-perf/src/clean-math-1/clean-math.ts
>>> time: 21 ms fileName: /ts-node-perf/src/clean-math-2/clean-math.spec.ts
>>> time: 3 ms fileName: /ts-node-perf/src/clean-math-2/clean-math.ts
>>> time: 20 ms fileName: /ts-node-perf/src/clean-math-3/clean-math.spec.ts
>>> time: 1 ms fileName: /ts-node-perf/src/clean-math-3/clean-math.ts
>>> time: 18 ms fileName: /ts-node-perf/src/clean-math-4/clean-math.spec.ts
>>> time: 2 ms fileName: /ts-node-perf/src/clean-math-4/clean-math.ts
>>> time: 29 ms fileName: /ts-node-perf/src/clean-math-5/clean-math.spec.ts
>>> time: 2 ms fileName: /ts-node-perf/src/clean-math-5/clean-math.ts
>>> time: 21 ms fileName: /ts-node-perf/src/clean-math-6/clean-math.spec.ts
>>> time: 1 ms fileName: /ts-node-perf/src/clean-math-6/clean-math.ts
>>> time: 19 ms fileName: /ts-node-perf/src/clean-math-7/clean-math.spec.ts
>>> time: 1 ms fileName: /ts-node-perf/src/clean-math-7/clean-math.ts
>>> time: 22 ms fileName: /ts-node-perf/src/clean-math-8/clean-math.spec.ts
>>> time: 2 ms fileName: /ts-node-perf/src/clean-math-8/clean-math.ts
>>> time: 14 ms fileName: /ts-node-perf/src/clean-math-9/clean-math.spec.ts
>>> time: 2 ms fileName: /ts-node-perf/src/clean-math-9/clean-math.ts
>>> time: 16 ms fileName: /ts-node-perf/src/clean-math/clean-math.spec.ts
>>> time: 1 ms fileName: /ts-node-perf/src/clean-math/clean-math.ts
```
