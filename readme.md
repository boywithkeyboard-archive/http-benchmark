## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72915` | `3859` | `81466` |
| **86%** | [Hyper Express](#hyper-express) | `62877` | `3292` | `71068` |
| **37%** | [Node (Default)](#node-default) | `27126` | `8123` | `56709` |
| **33%** | [Fastify](#fastify) | `24062` | `7058` | `38092` |
| **29%** | [Hono](#hono) | `21041` | `6132` | `30005` |
| **26%** | [Koa](#koa) | `18625` | `7112` | `58705` |
| **11%** | [Carbon](#carbon) | `8294` | `1384` | `10316` |
| **9%** | [Express](#express) | `6456` | `1061` | `8374` |


### In Detail

- #### Carbon
  [NPM](https://npmjs.com/@sinclair/carbon) | [GitHub](https://github.com/sinclairzx81/carbon)
  ```js
  import { listen } from '@sinclair/carbon/http'

  listen({
    hostname: '127.0.0.1',
    port: 3000
  }, () => {
    return new Response('Hello World', {
      status: 200,
      headers: {
        'content-type': 'text/plain'
      }
    })
  })
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec      8512.20    3736.22   49924.99
    Latency        5.87ms     4.37ms   375.66ms
    HTTP codes:
      1xx - 0, 2xx - 94856, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5144
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5144
    Throughput:     1.83MB/s
  ```

- #### Express
  [NPM](https://npmjs.com/express) | [GitHub](https://github.com/expressjs/express)
  ```js
  import express from 'express'

  const app = express()

  app.get('/', function (req, res) {
    res.send('Hello World')
  })

  app.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec      6324.00     969.09    8470.08
    Latency        7.90ms     3.66ms   353.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
  ```

- #### Fastify
  [NPM](https://npmjs.com/fastify) | [GitHub](https://github.com/fastify/fastify)
  ```js
  import fastify from 'fastify'

  const app = fastify({
    logger: false
  })

  app.get('/', (req, res) => {
    res.send('Hello World')
  })

  app.listen({ port: 3000 }, (err) => {
    if (err) throw err
  })
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     24641.95    7807.66   37112.00
    Latency        2.03ms     2.02ms   179.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.58MB/s
  ```

- #### Hono
  [NPM](https://npmjs.com/hono) | [GitHub](https://github.com/honojs/hono)
  ```js
  import { serve } from '@hono/node-server'
  import { Hono } from 'hono'

  const app = new Hono()

  app.get('/', (c) => c.text('Hello World'))

  serve(app)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     20943.59    5705.93   30112.03
    Latency        2.38ms     2.06ms   183.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
  ```

- #### Hyper Express
  [NPM](https://npmjs.com/hyper-express) | [GitHub](https://github.com/kartikk221/hyper-express)
  ```js
  import HyperExpress from 'hyper-express'

  const server = new HyperExpress.Server()

  server.get('/', (req, res) => {
    res.send('Hello World')
  })

  server.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     64037.84    2909.05   70002.95
    Latency      778.90us    62.70us     2.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.09MB/s
  ```

- #### Koa
  [NPM](https://npmjs.com/koa) | [GitHub](https://github.com/koajs/koa)
  ```js
  import Koa from 'koa'

  const app = new Koa()

  app.use(ctx => {
    ctx.body = 'Hello World'
  })

  app.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     18715.73    6543.97   57160.33
    Latency        2.67ms     2.33ms   210.56ms
    HTTP codes:
      1xx - 0, 2xx - 94470, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5530
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5530
    Throughput:     4.00MB/s
  ```

- #### Node (Default)
  [Website](https://nodejs.org/api/http.html)
  ```js
  import { createServer } from 'node:http'

  const server = createServer((req, res) => {
    res.writeHead(200, {
      'content-type': 'text/plain'
    })

    res.write('Hello World')

    res.end()
  })

  server.listen(3000, '127.0.0.1')
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     25353.76    7005.44   49038.89
    Latency        1.97ms     1.84ms   158.13ms
    HTTP codes:
      1xx - 0, 2xx - 97696, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2304
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2304
    Throughput:     5.67MB/s
  ```

- #### uWS
  [GitHub](https://github.com/uNetworking/uWebSockets.js)
  ```js
  import { App } from 'uWebSockets.js'

  const app = App()

  app.get('/', (res, req) => {
    res.end('Hello World')
  })

  app.listen(3000, () => {})
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     72974.67    5192.41   82716.85
    Latency      682.90us   142.06us     5.61ms
    HTTP codes:
      1xx - 0, 2xx - 97340, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2660
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2660
    Throughput:    11.24MB/s
  ```


