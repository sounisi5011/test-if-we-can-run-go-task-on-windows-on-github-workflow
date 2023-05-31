# Test if we can run our go-tasks on windows on GitHub Actions

Since [Windows Server 2022 (20230417) Image Update](https://github.com/actions/runner-images/releases/tag/win22%2F20230417.1), the following error occurs when running [go-task/task]:

[go-task/task]: https://github.com/go-task/task

```console
$ task test
This version of ...\task.exe is not compatible with the version of Windows you're running. Check your computer's system information and then contact the software publisher.
```

A common cause of this error is running a 32-bit application on 64-bit Windows. However, the binary I installed is `task_windows_amd64.zip`, so this is not the cause.

<del>Even more puzzling, this error occurs when installing on Node.js 18.16.0 using pnpm. Maybe it is not `task.exe`, maybe it is Node.js or pnpm, but I cannot figure out the cause.</del>

<del>The purpose of this repository is to investigate the cause of this problem.</del>

The reason for this is a bug in the [`@go-task/go-npm`] dependency [`unzipper`], which [go-task/task] uses [`@go-task/go-npm`] to install binaries.
Bug Details: https://github.com/ZJONSSON/node-unzipper/issues/271

[`@go-task/go-npm`]: https://github.com/go-task/go-npm
[`unzipper`]: https://www.npmjs.com/package/unzipper

This bug occurs in the following versions of Node.js:

- Node.js 18: `>=18.16.0 <19.0.0-0` ( 18.16.0 or later )
- Node.js 19: `>=19.8.0 <20.0.0-0` ( 19.8.0 or later )
- Node.js 20: `>=20.0.0 <21.0.0-0` ( All versions )

When [`@go-task/go-npm`] is run on these Node.js, [`unzipper`] destroys the binaries and installs incorrect data.
This will cause [go-task/task] to fail.

The bug is in the decompression process of the ZIP archive and has nothing to do with the OS type.
However, [go-task/task] releases as `.zip` files are only for Windows.
Therefore, this problem only occurs on Windows.
