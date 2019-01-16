# Setup guide
miu~~

## Prepare

Always start from builting the local TypeScript compiler.

1. Clone/Fork & Clone the TypeScript code base to local machine.

2. Run `jake local`.

3. Good to go.

## The easy way

1. Open a whatever dir says `debug-ts-complier` with file type relates to some features you want to experiment with TypeScript.

2. Create `.vscode/launch.json` file in TypeScript codebase, with setting like the following:

```json
{
  "type": "node",
  "request": "launch",
  "name": "Launch Program",
  "program": "${workspaceFolder}/built/local/tsc.js",
  "args": [
      "/path/to/debug-ts-compiler/file.whatever.type",
      "--skipLibCheck",
      "--noLib"
  ],
  "sourceMaps": true,
  "protocol": "inspector"
}
```

3. Start debugging with breakpoints you want to experiment.

There are some shortcuts with this method, like no interaction with the compiler when typing or hovering etc.

## The ~~hard~~ way

Actually this method is easy as well, but the official document confuses me a lot (of caurse my english's bad) and I searched google with `debug TypeScript compiler` gives me unrelavant results, it cost me almost a day to figure it out.

1. Build a local copy of development version of vscode called `Code OSS`.

2. Setup the `tsdk` in`.vscode/setting.json` in the built version vscode `Code OSS`.

```json
{
    "typescript.tsdk": "/path/to/TypeScript/built/local"
}
```

3. Setup environment variable `TSS_DEBUG` as a port. `Code OSS` instance will runs on this port when run the command `./scripts/code.sh` in vscode codebase.

Run the following code in a terminal.

```js
export TSS_DEBUG=5859
```

4. Select the TypeScript version to the above path by clicking right corner in `Code OSS`.

5. Add configuration below to `.vsocode/launch.json` in TypeScript codebase. It tells the vscode debugger in TypeScript codebase to attach to the port where `Code OSS` is running.

```json
{
  "name": "Attach To Code OSS",
  "type": "node",
  "request": "attach",
  "outFiles": ["/path/to/TypeScript/built/local"],
  "sourceMaps": true,
  "port": 5859
},
```

There are some weird things happen when I try to use the official method below. The breakpoint in TypeScript codebase becomes gray with error `Breakpoint ignored because generated code not found (source map problem?)`, and I can't figure out what is going wrong...

```json
{
  "version": "0.2.0",
  "configurations": [
    // Other configs
    {
      "name": "Attach to TS Server",
      "type": "node",
      "request": "launch",
      "protocol": "inspector",
      "port": 5859,
      "sourceMaps": true,
      "outFiles": ["/path/to/repo/TypeScript/built/local"],
      "runtimeArgs": [
          "--inspect=5859"
      ]
    }
  ]
}
```

6. Debug with `Attach To Code OSS` and happy coding.

NOTES: If it doesn't work, maybe you need to switch the order between starting a debugger and building the `Code OSS` instance, reason unkonwn...

## References

1. [Debugging Language Service in VS Code](https://github.com/Microsoft/TypeScript/wiki/Debugging-Language-Service-in-VS-Code)

2. [How TypeScript team debugs typescript compiler code?](https://github.com/Microsoft/TypeScript/issues/24943)

3. [Setup a development version of VS Code](https://github.com/Microsoft/vscode/wiki/How-to-Contribute)

