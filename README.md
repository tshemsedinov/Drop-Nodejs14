## Drop Node.js 14 support

- Now: Apr 2023, we have node.js 18.16.0 LTS, and 16.20.0 (maintenance) and 20.0.0 (latest);
- Going to drop Node.js 14 support in all HowProgrammingWorks and Node.js courses, Metarhia codebase;
- Now we can rely on node.js 16.20.0 capabilities;
- See release schedule: https://github.com/nodejs/release#release-schedule;

### Refactoring checklist moving to latest node.js 16 in 2023

- Due to move from `npm 6` to `npm 8` convert `package_lock.json` to `lockfileVersion` version `2`;
- Now we can use true `#privateField` syntax instead of `_pseudoPrivateField` or closure for "Information Hiding" principle;
- Change deprecated `cluster.isMaster` to `cluster.isPrimary`, change `cluster.setupMaster` to `cluster.setupPrimary`;
- Stop using `fs.exists` use `fs.stat` or `fs.access` instead;
- Stop using `fs.rmdir(path, { recursive: true }`, use `fs.rm(path, { recursive: true }` instead;
- Check `stream.destroyed` instead of the `.aborted` property, and listen for `close` event instead of `abort` and `aborted` events for `ClientRequest`, `ServerResponse`, and `IncomingMessage`;
- Stop using deprecated `Server.connections`;

### Explore new features available in all currently supported node.js versions

- Take a look at [Diagnostics Channel API](https://nodejs.org/api/diagnostics_channel.html)
- Use `error.cause` to [chain errors](https://nodejs.org/api/errors.html#errorcause) like: `new Error('message', { cause: error })`;
- Use `AbortSignal` in different async I/O APIs;
- Use `node:dns/promises`;
- Use `node:timers/promises`, for example `await setTimeout(100)`;
- Use `node:assert/strict`;
- Use `WeakReferences` and `FinalizationRegistry`;
- Use new `Promise` method `any`;
