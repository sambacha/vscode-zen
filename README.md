# vscode-zen
vscode settings that are interoperable and sane

## Debugging TypeScript with ts-node in VSCode
These launch configs will allow you to debug typescript files directly from VSCode. It will honor the `tsconfig` and resolve node modules properly. You do not need to install `ts-node` or `nodemon`, as everything is run using `npx`. The first 

### Launch Config
```json
{
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "ts-node - current file",
      "runtimeExecutable": "npx",
      "runtimeArgs": ["-y", "ts-node"],
      "program": "${file}",
      "cwd": "${fileDirname}",
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen"
    },
    {
      "type": "node",
      "request": "launch",
      "name": "ts-node - watch current file",
      "runtimeExecutable": "npx",
      "runtimeArgs": [
        "-y",
        "nodemon",
        "-q",
        "--exec",
        "npx",
        "-y",
        "ts-node"
      ],
      "program": "${file}",
      "cwd": "${fileDirname}",
      "restart": true,
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen"
    },
  ],
}
```

## Explanation
The first one is pretty straightforward. If the file is `src/path/to/example.ts`, this is the equivalent of:
```
cd src/path/to
npx -y ts-node example.ts
```
It's better to use `cwd` in this way, as this allows for nested `tsconfig` files as may appear in a monorepo.

The second one had to be a bit sneaky because of quirks in `nodemon`. They recently added TypeScript support, but internally it simply runs the `ts-node` command instead of `node`. However, to avoid the dependency for those using regular JS, they didn't add `ts-node` to its dependencies, so `npx nodemon` doesn't supply the dependency. You could avoid this by installing it in the project with `npm i -D ts-node`, but I wanted these to work without installing anything. This is solved with the `--exec` option by compounding `npx` scripts. The second config is equivalent to:
```
cd src/path/to
npx -y nodemon -q --exec 'npx -y ts-node' ./example.ts
```
