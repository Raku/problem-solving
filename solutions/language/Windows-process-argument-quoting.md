Windows process argument quoting
================================

Windows does not have the concept of an ARGV array. At the API level ([CreateProcess()](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessw)) there is only one single string that is passed. There is a [convention](https://docs.microsoft.com/en-us/previous-versions/17w5ykft(v=vs.85)) ([CommandLineToArgv()](https://docs.microsoft.com/en-us/windows/desktop/api/shellapi/nf-shellapi-commandlinetoargvw) implements that) of how a string array is serialized to a single string and back. With rare exceptions all programs adhere to this convention. The most prominent exception is `cmd.exe`. `cmd.exe` just directly processes its argument string as if you'd have pasted it into a cmd.exe window. When calling a `.bat` file, Windows implicitly wraps the call in `cmd.exe /C <your.bat stuff>`. (The exact command Windows actually puts around the command is currently unknown.) Thus calling any `.bat` file is subject to `cmd.exe` processing.

So in most cases arguments need to be serialized using the Microsoft convention. In some cases, usually when dealing with `.bat` files, one needs to serialize the arguments differently though.

All APIs in Raku to call processes do always quote arguments using the Microsoft convention and provide no escape hatch to disable the quoting. So it's impossible to call processes with a different serialization format.

A good solution should:

- Work in the common case without the user having to know about any of this
- Give enough control to the user to call any program the way he wants
- Make sure the changes don't make the API more complex for the use cases not affected by this


Solution
--------

`sub run()`, `Proc.Spawn()` and `Proc::Async.new()` are extended with a new argument `:$win-verbatim-args` defaulting to `False`. If the argument is left off or set to `False`, the arguments are quoted according to the Microsoft convention. This is identical to Rakus previous behavior and thus backwards compatible.

When passing `True` the passed arguments are concatenated with a single space. No other processing takes place. This allows to manually quote the arguments as necessary.

Other serialization strategies can be implemented in ecosystem modules.

The name `win-verbatim-args` is named similar to a matching flag in `libuv` and `node.js`. This increases the recognition factor.


Implementation in Rakudo
------------------------

The serialization of the arguments happens in Rakudo. Backends are expected to not touch the single arument string they receive from Rakudo on Windows in any way.

The JVM does not provide any mechanism to disable or influence the argument quoting in any way. It does however implement a sophisticated heuristic to guess which quoting the user might want and acts accordingly. So on the JVM backend the above described flag is ignored and arguments are passed as separate strings to the JVM backend.


Further reading
---------------

- The problem-solving ticket: [Raku/problem-solving#20](https://github.com/Raku/problem-solving/issues/20)
- Older bugreport on this topic: [rakudo/rakudo#2005](https://github.com/rakudo/rakudo/issues/2005)
- Writeup of how commandline processing works on Windows: [Everyone quotes command line arguments the wrong way](https://web.archive.org/web/20190109172835/https://blogs.msdn.microsoft.com/twistylittlepassagesallalike/2011/04/23/everyone-quotes-command-line-arguments-the-wrong-way/).
- Documentation of the Windows API function to parse command line arguments: [CommandLineToArgv()](https://docs.microsoft.com/en-us/windows/desktop/api/shellapi/nf-shellapi-commandlinetoargvw)


### What other languages do

[libuv](https://github.com/libuv/libuv/blob/c5593b51dc98715f7f32a919301b5801ebf1a8ce/src/win/process.c#L593) quotes arguments compatible to `CommandLineToArgv()`. When passing the `UV_PROCESS_WINDOWS_VERBATIM_ARGUMENTS` argument this quoting is disabled and the arguments are just joined with a space.

[Node.js](https://github.com/nodejs/node/blob/382e859afc7e66600dccfadd4125088444e063c3/lib/child_process.js#L486) exposes a `shell` flag which wraps the call in `cmd.exe /d /s /c "<command>"` by hand and enables `UV_PROCESS_WINDOWS_VERBATIM_ARGUMENTS`.

[Java](https://codewhitesec.blogspot.com/2016/02/java-and-command-line-injections-in-windows.html)s `ProcessBuilder` has a very sophisticated and complex processing of arguments. e.g. it detects whether the user called a `.bat` file or `.cmd` file and acts on that.