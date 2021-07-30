## Prerequisites

```
# Enable emscripten tools
$ source <path-to-emsdk-repo>/emsdk_env.sh

# Set working directory to src/emscripten
$ cd src/emscripten

# Optional (headless testing, vercel cli, etc...)
$ npm install
```

## Build

```
$ make -C .. emscripten_build ARCH=wasm # or equivalently `npm run build`
```

## Run

- Node

```
# Check node version
$ node -e 'console.log(`node: ${process.version}\nv8: ${process.versions.v8}`)'
node: v16.5.0
v8: 9.1.269.38-node.20

# UCI console
$ node public/uci.js
```

- Browser

```
# Start server for http://localhost:5000/test-puppeteer.html
$ npm run serve

# Run UCI command inside of headless browser
$ npm install puppeteer
$ node public/uci-puppeteer.js bench # not interactive
```

## Build/Run in Docker

```
# Build
$ DOCKER_USER=$(id -u):$(id -g) docker-compose run emscripten bash
> make -C .. emscripten_build ARCH=wasm

# Run Node
$ docker-compose run node bash
> node public/uci.js

# Run headless browser
$ docker-compose run browser bash
> node public/uci-puppeteer.js bench
```

## Testing

Cf. `.github/workflows/ci.yml`

```
$ UCI_EXE="node public/uci.js"           bash tests/bench-nps.sh
$ UCI_EXE="node public/uci.js"           bash tests/bench-signature.sh
$ UCI_EXE="node public/uci.js"           bash tests/stress.sh

$ UCI_EXE="node public/uci-puppeteer.js" bash tests/bench-nps.sh
$ UCI_EXE="node public/uci-puppeteer.js" bash tests/bench-signature.sh
$ UCI_EXE="node public/uci-puppeteer.js" bash tests/stress.sh
```

## Misc

- Publish npm package

```
$ npm run build:refresh
$ npm publish ./public
```
