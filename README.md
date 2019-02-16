# rush-webpack-repro
Simple project that exhibits an issue with Webpack &amp; Rush

## What's wrong?

- Webpack does not use stderr (which is a pity), errors are printed to stdout
- `rush build`, when the build tool fails (exits with non-zero status), will print tool's stderr
- So, `rush build` does not show Webpack errors, you have to use `rush build -v`.

Example with this project (clone, then `rush update`, then `rush build`):

    $ rush build
    Rush Multi-Project Build Tool 5.5.4 - https://rushjs.io
    Starting "rush build"
    Executing a maximum of 8 simultaneous processes...
    [foo] started
    0 of 1: [foo] failed to build!
    FAILURE (1)
    ================================
    foo (1.36 seconds)
    ================================
    [foo] Returned error code: 2
    Error: Project(s) failed to build
    rush build - Errors! (1.41 seconds)

Great, it fails, but no idea why. I have to do `rush build -v` to see this:

    ERROR in ./src/index.js
    Module not found: Error: Can't resolve 'nope-nope-nope' in '/home/nug/dev/xxx/rush-webpack-repro/foo/src'
    @ ./src/index.js 1:0-25
    
## Suggestion

In case of build failure, if stderr is empty, could Rush print stdout instead?

One could argue that Webpack needs fixing, and I would agree, but I bet that Webpack is not the only one to behave that way, so IMHO it would make sense to do something in Rush.
