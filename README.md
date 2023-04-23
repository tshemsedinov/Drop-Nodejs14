## Drop Node.js 14 support

- Now: Apr 2023, we have node.js 18.16.0 LTS, and 16.20.0 (maintenance) and 20.0.0 (latest)
- Going to drop Node.js 14 support in all HowProgrammingWorks and Node.js courses, Metarhia codebase
- Now we can rely on node.js 16.20.0 capabilities
- See release schedule: https://github.com/nodejs/release#release-schedule

### Refactoring checklist moving to latest node.js 16 in 2023

- Due to move from `npm 6` to `npm 8` convert `package_lock.json` to `lockfileVersion` version `2`
- Now we can use true `#privateField` syntax instead of `_pseudoPrivateField` or closure for "Information Hiding" principle
- Change deprecated `cluster.isMaster` to `cluster.isPrimary`, change `cluster.setupMaster` to `cluster.setupPrimary`
- Stop using `fs.exists` use `fs.stat` or `fs.access` instead
- Stop using `fs.rmdir(path, { recursive: true }`, use `fs.rm(path, { recursive: true }` instead, see docs: https://nodejs.org/api/fs.html#fsrmpath-options-callback
- Check `stream.destroyed` instead of the `.aborted` property, and listen for `close` event instead of `abort` and `aborted` events for `ClientRequest`, `ServerResponse`, and `IncomingMessage`
- Stop using deprecated `Server.connections`, use `Server.getConnections()` instead, see docs: https://nodejs.org/api/net.html#servergetconnectionscallback
- Do not use default `index.js`, as of node.js 16.0.0 main entry point should be explicitly defined
- Stop using `emitter.listeners`, use `events.getEventListeners(emitter, eventName)` instead, , see docs: https://nodejs.org/api/events.html#eventsgeteventlistenersemitterortarget-eventname
- Now `response` (http.ServerResponse) have reference to `request` instance (http.IncomingMessage): `response.req`
- Stop using `node:url` API, use JavaScript URL class instead
- Note that unhandled promise rejections are deprecated and will terminate process with a non-zero exit code. You need to add `process.on('unhandledRejection', (reason, promise) => {})` handler to prevent process termination
- Note that `--watch` is still experimental
- Note that `fs.cp` and `fsPromises.cp` are still experimental
- Stop using deprecated `process.config`
- Check for deprecated Thenable support in streams
- Stop using deprecated `process.on('multipleResolves', handler)`
- Check to use only integers `process.exit(code)` and `process.exitCode`
- The well-known MODP groups modp1, modp2, and modp5 are deprecated because they are not secure against practical attacks

### Explore new features available in all currently supported node.js versions

- Take a look at Diagnostics Channel API: https://nodejs.org/api/diagnostics_channel.html
- Use `error.cause` to chain errors like: `new Error('message', { cause: error })`, see docs: https://nodejs.org/api/errors.html#errorcause
- Use `AbortController` and `AbortSignal` in different async I/O APIs: `exec/fork/spawn`, `events.once`, `readFile`, `stream.Writable/Readable` and so on
- Use `AsyncLocalStorage`, `AsyncResource` see docs: https://nodejs.org/api/async_context.html#class-asynclocalstorage
- Use multiple new promisified APIs: `node:stream/promises`, `node:dns/promises`, `node:assert/strict`
- Use `node:timers/promises`, for example `await setTimeout(100)`
- Use `WeakReferences` and `FinalizationRegistry`
- Use new `Promise` method `any`
- Discoder multiple improvements in `node:crypto`
- Discover multiple improvements in Event API: `Event` and `EventTarget` classes: https://nodejs.org/api/events.html#eventtarget-and-event-api
- Discover class `BlockList` from `node:net`: https://nodejs.org/api/net.html#class-netblocklist
- Discover perf_hooks.createHistogram: https://nodejs.org/api/perf_hooks.html
- Discover new Web Crypto API: https://nodejs.org/api/webcrypto.html
- Discover `BroadcastChannel` and `MessagePort` from https://nodejs.org/api/worker_threads.html
- Discover Node.js native test runner from `node:test`: https://nodejs.org/api/test.html
