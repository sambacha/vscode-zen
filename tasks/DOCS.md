{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Example",
      "type": "node",
      "request": "launch",
      "runtimeExecutable": "node",
      "runtimeArgs": ["--nolazy", "-r", "ts-node/register/transpile-only"],

      "args": ["src/script.ts", "--example", "hello"],
      
      "cwd": "${workspaceRoot}",
      "internalConsoleOptions": "openOnSessionStart",
      "skipFiles": ["<node_internals>/**", "node_modules/**"]
    }
  ]
}

    "dev": "tsnd --files --transpile-only --respawn --inspect=4000 --project dev.tsconfig.json index.ts node-dev",
