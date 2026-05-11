## Background

Contribute code to https://github.com/aptakube/kubespec.dev and run it locally using `npm run dev`.

Page loading is extremely, extremely, extremely slow.

Try running the command `npm run build`.

## Question

Encountering an OOM when running `npm run build`.

```bash
$ npm run build

> kubespec-dev@0.0.0 dev
> astro dev

22:13:56 [types] Generated 0ms
22:13:56 [content] Syncing content
22:13:56 [content] Synced content

 astro  v5.12.3 ready in 272 ms

┃ Local    http://localhost:4321/
┃ Network  use --host to expose

22:13:56 watching for file changes...

<--- Last few GCs --->

[307400:0x1c8f4000]   292392 ms: Scavenge 4007.2 (4128.0) -> 3993.4 (4129.5) MB, pooled: 5 MB, 5.33 / 0.00 ms  (average mu = 0.181, current mu = 0.011) allocation failure;
[307400:0x1c8f4000]   292956 ms: Mark-Compact 4009.3 (4130.2) -> 3973.8 (4129.4) MB, pooled: 4 MB, 552.03 / 0.00 ms  (average mu = 0.239, current mu = 0.297) allocation failure; scavenge might not succeed


<--- JS stacktrace --->

FATAL ERROR: Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory
----- Native stack trace -----

 1: 0xe36068 node::OOMErrorHandler(char const*, v8::OOMDetails const&) [node]
 2: 0x1202550 v8::Utils::ReportOOMFailure(v8::internal::Isolate*, char const*, v8::OOMDetails const&) [node]
 3: 0x1202827 v8::internal::V8::FatalProcessOutOfMemory(v8::internal::Isolate*, char const*, v8::OOMDetails const&) [node]
 4: 0x1430105  [node]
 5: 0x1430133  [node]
 6: 0x144920a  [node]
 7: 0x144c3d8  [node]
 8: 0x1cb2241  [node]
fish: Job 1, 'npm run dev' terminated by signal SIGABRT (Abort)
```

## Answer

```bash
NODE_OPTIONS="--max-old-space-size=16384" npm run build
```

Ref: https://stackoverflow.com/a/59572966/20973039