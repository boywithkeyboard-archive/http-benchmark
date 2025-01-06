## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73114` | `5364` | `80072` |
| **85%** | [Hyper Express](#hyper-express) | `61983` | `3402` | `64862` |
| **34%** | [Node (Default)](#node-default) | `25096` | `7330` | `42729` |
| **32%** | [Fastify](#fastify) | `23705` | `6980` | `35707` |
| **29%** | [Hono](#hono) | `21203` | `5790` | `30228` |
| **24%** | [Koa](#koa) | `17762` | `6454` | `53967` |
| **11%** | [Carbon](#carbon) | `8321` | `1423` | `10584` |
| **9%** | [Express](#express) | `6396` | `986` | `8397` |


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
    Reqs/sec      8773.08    4049.94   53381.48
    Latency        5.68ms     4.14ms   360.07ms
    HTTP codes:
      1xx - 0, 2xx - 94011, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5989
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5989
    Throughput:     1.88MB/s
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
    Reqs/sec      6320.16     953.91    8483.35
    Latency        7.91ms     3.50ms   337.44ms
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
    Reqs/sec     25010.70    7901.35   36260.43
    Latency        2.00ms     1.95ms   176.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.67MB/s
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
    Reqs/sec     20930.76    6447.24   30385.23
    Latency        2.39ms     2.02ms   182.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     63590.38    4923.81   82594.71
    Latency      783.57us    74.93us     3.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.04MB/s
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
    Reqs/sec     18379.83    6350.10   56032.46
    Latency        2.71ms     2.33ms   202.33ms
    HTTP codes:
      1xx - 0, 2xx - 94542, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5458
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5458
    Throughput:     3.94MB/s
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
    Reqs/sec     25509.57    7338.62   48902.57
    Latency        1.96ms     1.85ms   159.47ms
    HTTP codes:
      1xx - 0, 2xx - 97714, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2286
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2286
    Throughput:     5.70MB/s
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
    Reqs/sec     74769.17    5936.25   83427.95
    Latency      666.16us   153.34us     7.29ms
    HTTP codes:
      1xx - 0, 2xx - 97520, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2480
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2480
    Throughput:    11.54MB/s
  ```


