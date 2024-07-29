## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73005` | `10825` | `94697` |
| **88%** | [Hyper Express](#hyper-express) | `64280` | `2767` | `70948` |
| **36%** | [Node (Default)](#node-default) | `26036` | `8365` | `48324` |
| **32%** | [Fastify](#fastify) | `23402` | `6615` | `34779` |
| **28%** | [Hono](#hono) | `20298` | `5595` | `29564` |
| **24%** | [Koa](#koa) | `17478` | `5865` | `49913` |
| **11%** | [Carbon](#carbon) | `8105` | `1398` | `10246` |
| **9%** | [Express](#express) | `6340` | `976` | `8422` |


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
    Reqs/sec      8570.38    5084.01   62952.13
    Latency        5.83ms     4.18ms   358.44ms
    HTTP codes:
      1xx - 0, 2xx - 91615, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8385
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8385
    Throughput:     1.78MB/s
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
    Reqs/sec      6521.00    1032.79    8428.64
    Latency        7.66ms     3.61ms   347.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     23032.07    6596.94   34936.34
    Latency        2.17ms     1.99ms   175.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.23MB/s
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
    Reqs/sec     20807.05    5715.24   29381.31
    Latency        2.40ms     1.91ms   174.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     64386.85    4718.66   92581.16
    Latency      778.79us    67.27us     2.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.10MB/s
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
    Reqs/sec     18385.99    6530.87   61587.96
    Latency        2.71ms     2.38ms   211.81ms
    HTTP codes:
      1xx - 0, 2xx - 94206, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5794
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5794
    Throughput:     3.92MB/s
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
    Reqs/sec     25778.76    7821.62   56292.15
    Latency        1.92ms     1.92ms   161.80ms
    HTTP codes:
      1xx - 0, 2xx - 97369, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2631
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2631
    Throughput:     5.78MB/s
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
    Reqs/sec     73744.15    5982.75   83263.63
    Latency      675.06us   192.04us     9.08ms
    HTTP codes:
      1xx - 0, 2xx - 98081, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1919
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1919
    Throughput:    11.45MB/s
  ```


