# Test if we can run our go-tasks on windows on GitHub Actions

Since [Windows Server 2022 (20230417) Image Update](https://github.com/actions/runner-images/releases/tag/win22%2F20230417.1), the following error occurs when running [go-task/task](https://github.com/go-task/task):

```console
$ task test
This version of ...\task.exe is not compatible with the version of Windows you're running. Check your computer's system information and then contact the software publisher.
```

A common cause of this error is running a 32-bit application on 64-bit Windows. However, the binary I installed is `task_windows_amd64.zip`, so this is not the cause.

Even more puzzling, this error occurs when installing on Node.js 18.16.0 using pnpm. Maybe it is not `task.exe`, maybe it is Node.js or pnpm, but I cannot figure out the cause.

The purpose of this repository is to investigate the cause of this problem.
