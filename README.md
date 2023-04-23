# Drop Node.js 14 support

## Refactoring checklist moving to latest node.js 16 in 2023

- Due to move fron `npm 6` to `npm 8` convert `package_lock.json` to `lockfileVersion` version `2`;
- Now we can use true `#privateField` syntax instead of `_pseudoPrivateField` or closure for "Information Hiding" principle;
- Use new `Promise` method `any`;
- Change deprecated `cluster.isMaster` to `cluster.isPrimary`, change `cluster.setupMaster` to `cluster.setupPrimary`;
- Stop uning `fs.exists` use `fs.stat` or `fs.access` instead;
- Stop using `fs.rmdir(path, { recursive: true }`, use `fs.rm(path, { recursive: true }` instead;
- Check `stream.destroyed` instead of the `.aborted` property, and listen for `close` event instead of `abort` and `aborted` events for `ClientRequest`, `ServerResponse`, and `IncomingMessage`;
- Use `node:timers/promises`, for example `await setTimeout(100)`;
- Use `node:assert/strict`;
- Use `AbortSignal` in different async I/O APIs;
- Stop using deprecated `Server.connections`;
- Use `WeakReferences` and `FinalizationRegistry`;
