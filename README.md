# OCR Application based on penteract module 

Testing the penteract-ocr node module with electron

## Installation

run `npm i`

## Run

run 
`npm run start-dev` in one terminal to start webpack and
`npm run start-electron` to start electron (will fetch the page from webpack)


# Current problems:

## 1. Penteract can not be loaded

See https://github.com/kaelzhang/penteract-ocr/issues/11

When starting electron dev server via `npm run start-dev` you will get a stacktrace similar to this:
```
  ERROR in ./node_modules/penteract/src/index.js
  Module not found: Error: Can't resolve '../build/Release/penteract' in '/Users/steph.../electron-penteract/node_modules/penteract/src'
  @ ./node_modules/penteract/src/index.js 1:0-49 25:4-12
  @ ./app/App.js
  @ ./app/main.js
  @ multi @babel/polyfill react-hot-loader/patch webpack-dev-server/client?http://localhost:3000 webpack/hot/only-dev-server ./app/main.js
```

I figured out that by changing the file electron-penteract/node_modules/penteract/src/index.js I can fix the error:

changing the line
`import bindings from '../build/Release/penteract'`
to
`import bindings from '../build/Release/penteract.node'`

## 2. Penteract can not be loaded in production

It seems that for some reason when packaging the app via `npm run export-mac`, the path to the penteract module get's hardcoded. As soon as I move the .app file to another computer and execute it, I get following error:

```Uncaught Error: Cannot open /Users/steph/Documents/workspace/electron-penteract/node_modules/penteract/build/Release/penteract.node: Error: dlopen(/Users/steph/Documents/workspace/electron-penteract/node_modules/penteract/build/Release/penteract.node, 1): image not found
    at Object.<anonymous> (penteract.node:1)
    at Object../node_modules/penteract/build/Release/penteract.node (main-bundle.js:45)
    at n (bootstrap:19)
    at Module../node_modules/penteract/src/index.js (index.js:1)
    at n (bootstrap:19)
    at i.testPenteract (App.js:45)
    at onClick (App.js:65)
    at Object.i (react-dom.production.min.js:14)
    at m (react-dom.production.min.js:14)
    at react-dom.production.min.js:14
  ```

  **The path to the .node file should not be hardcoded. I guess this is related to the fix of problem #1 (see above).**
