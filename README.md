## Drop Node.js 14 support in your projects

- Now: April 2023, we have Node.js 18.16.0 LTS, 16.20.0 (maintenance) and 20.0.0 (latest)
- Going to drop Node.js 14 support in all HowProgrammingWorks and Node.js courses, Metarhia codebase
- Now we can rely on Node.js 16.20.0 capabilities
- See release schedule: https://github.com/nodejs/release#release-schedule

### Refactoring checklist as of April 2023

- Due to the move from `npm 6` to `npm 8`, convert `package_lock.json` to `lockfileVersion 2`
- Now we can use true `#privateField` syntax instead of `_pseudoPrivateField` or closures for "Information Hiding" principle
- Change deprecated `cluster.isMaster` to `cluster.isPrimary`, change `cluster.setupMaster` to `cluster.setupPrimary`
- Check all `require` calls to use `node:` prefix for all internal node.js modules, for example: `node:fs`
- Use `crypto.randomUUID()` to generate UUID instead of manual generation or third-party modules
- Stop using `fs.exists`; use `fs.stat` or `fs.access` instead, see example:
```js
const toBool = [() => true, () => false];
const exists = await fs.promises.access(filePath).then(...toBool);
```
- Stop using `fs.rmdir(path, { recursive: true })`; use `fs.rm(path, { recursive: true })` instead, see docs: https://nodejs.org/api/fs.html#fsrmpath-options-callback
- Check `stream.destroyed` instead of the `.aborted` property, and listen for `close` event instead of `abort` and `aborted` events for `ClientRequest`, `ServerResponse`, and `IncomingMessage`
- Stop using deprecated `Server.connections`; use `Server.getConnections()` instead, see docs: https://nodejs.org/api/net.html#servergetconnectionscallback
- Do not use default `index.js` as of Node.js 16.0.0; the main entry point should be explicitly defined
- Stop using `emitter.listeners` property; use `emitter.listeners(eventName)` and `events.getEventListeners(emitter, eventName)` instead to get copy of listeners collection, see docs: https://nodejs.org/api/events.html#eventsgeteventlistenersemitterortarget-eventname
- Now `response` (http.ServerResponse) has a reference to `request` instance (http.IncomingMessage): `response.req`
- Stop using `node:url` API; use JavaScript URL class instead
- Note that unhandled promise rejections are deprecated and will terminate the process with a non-zero exit code. Add a `process.on('uncaughtException', (reason, origin) => {})` handler to prevent process termination
- Stop using deprecated `process.on('multipleResolves', handler)`
- Don't change `process.config` (frozen)
- Check for deprecated `Thenable` support in streams
- Ensure only integer values are used for `process.exit(code)` and `process.exitCode`
- The well-known MODP groups modp1, modp2, and modp5 are deprecated due to their lack of security against practical attacks
- Use `http.createServer({ noDelay, keepAlive, keepAliveInitialDelay })`, no need in `request.setNoDelay` anymore
- New `os.machine()` returns one of machine types as a string: `arm`, `arm64`, `aarch64`, `mips`, `mips64`, `ppc64`, `ppc64le`, `s390`, `s390x`, `i386`, `i686`, `x86_64`

### Explore new features

Available in all currently supported Node.js versions as of April 2023

- Use `error.cause` to chain errors like: `new Error('message', { cause: error })`, see docs: https://nodejs.org/api/errors.html#errorcause
- Use `AbortController` and `AbortSignal` in different async I/O APIs: `exec/fork/spawn`, `events.once`, `readFile`, `stream.Writable/Readable`, and so on
- Use `AsyncLocalStorage`, `AsyncResource`, see docs: https://nodejs.org/api/async_context.html
- Use multiple new promisified APIs: `node:stream/promises`, `node:dns/promises`, `node:assert/strict`
- Use `node:timers/promises`, for example `await setTimeout(100)`
- Use `WeakReferences` and `FinalizationRegistry`
- Use new `Promise` method `any`
- Use `node:http` improvements: `server.maxRequestsPerSocket`, `response.strictContentLength`, `message.headersDistinct`, `message.trailersDistinct`, `outgoingMessage.appendHeader`, `http.setMaxIdleHTTPParsers`, see docs: https://nodejs.org/api/http.html
- Discover multiple improvements in `node:crypto`
- Discover multiple improvements in Event API: `Event` and `EventTarget` classes: https://nodejs.org/api/events.html#eventtarget-and-event-api
- Discover class `BlockList` from `node:net`: https://nodejs.org/api/net.html#class-netblocklist
- Discover perf_hooks improvements like `createHistogram`, `PerformanceMark`, `getEntries`, `PerformanceMeasure`, `PerformanceResourceTiming`, multiple changes of `PerformanceObserver` and `Histogram`, new `perf_hooks.performance`: https://nodejs.org/api/perf_hooks.html
- Discover new Web Crypto API: https://nodejs.org/api/webcrypto.html
- Discover new `node:worker_threads` features `getEnvironmentData` and `setEnvironmentData`, new classes `BroadcastChannel`, `EventTarget`, and `MessagePort`, see docs: https://nodejs.org/api/worker_threads.html
- Discover Node.js native test runner from `node:test`: https://nodejs.org/api/test.html
- Take a look at the Diagnostics Channel API: https://nodejs.org/api/diagnostics_channel.html
- See `node:net` changes: `Server` event `drop`, `socket.connect` options: `noDelay`, `keepAlive`, and `keepAliveInitialDelay`, `socket.localFamily`, and `socket.resetAndDestroy`
- Discover Promise hooks API from `node:v8`m see docs: https://nodejs.org/api/v8.html#promise-hooks

### Note that you can't freely use

- Following features are still experimental in at least one of supported node.js versions `--watch`, `process.getActiveResourcesInfo`, `fs.cp`, and `fsPromises.cp`, `Readable` methods `map`, `filter`, `compose`, `iterator`, `forEach` and so on
- Fetch API is experimental in node.js 16.x and 17.x, and available without `--no-experimental-fetch` flag only from 18.0 and above, use `undici` from npm for previous node.js versions: https://github.com/nodejs/undici
- ESM and CJS loaders API is currently being redesigned and will still change
- Startup Snapshot API and Web Streams API are still experimental, see docs: https://nodejs.org/api/v8.html#startup-snapshot-api

### Use node.js features instead of dependencies

- Use `nodejs/undicu` instead of npm modules `request`, `axios`, `node-fetch`
- Prefer to use `node:child_process` and `node:worker_trheads` for CPU utilization and multithreading instead of `node:cluster`
- Prefer to use npm module `ws` + browser native implementation of `Websocket` instead of `socket.io`
- Use native `node:crypto.script` for password hashing or `argon2` from npm instead of `bcrypt` or other npm modules
- Use `node:async_hooks` instead of `zone.js` or deprecated built-in node.js `domain` module
- Prefer to use `docker` instead of `pm2` or `forever` npm modules
- Prefer to use `fastify` or native `node:http` + collection of routes `Map<string, Function>` instead of archaic `express`, `connect`, `koa`
- Use `Intl` and ES6 built-in features instead of `moment.js`
- Use `${templateStrings}` instead of `ejs` or other npm modules for tamplating
