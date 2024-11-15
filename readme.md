## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74013` | `5491` | `81775` |
| **86%** | [Hyper Express](#hyper-express) | `63560` | `4084` | `70895` |
| **35%** | [Node (Default)](#node-default) | `25738` | `7354` | `52986` |
| **32%** | [Fastify](#fastify) | `23934` | `6762` | `36002` |
| **28%** | [Hono](#hono) | `20693` | `5226` | `29127` |
| **24%** | [Koa](#koa) | `17672` | `6264` | `53685` |
| **11%** | [Carbon](#carbon) | `8311` | `1420` | `13039` |
| **9%** | [Express](#express) | `6545` | `1024` | `8606` |


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
    Reqs/sec      8816.81    4667.23   71296.33
    Latency        5.65ms     4.11ms   354.21ms
    HTTP codes:
      1xx - 0, 2xx - 93110, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6890
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6890
    Throughput:     1.87MB/s
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
    Reqs/sec      6557.48    1034.37    8586.25
    Latency        7.62ms     3.61ms   342.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     23831.83    7043.26   36141.46
    Latency        2.10ms     1.95ms   180.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.40MB/s
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
    Reqs/sec     20778.59    5413.21   29297.97
    Latency        2.41ms     1.82ms   168.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     63407.50    4019.31   73304.04
    Latency      786.49us    69.79us     2.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     18004.56    6173.79   50397.77
    Latency        2.77ms     2.44ms   225.34ms
    HTTP codes:
      1xx - 0, 2xx - 94485, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5515
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5515
    Throughput:     3.85MB/s
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
    Reqs/sec     25967.15    7449.27   55358.80
    Latency        1.92ms     1.92ms   166.30ms
    HTTP codes:
      1xx - 0, 2xx - 97362, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2638
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2638
    Throughput:     5.79MB/s
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
    Reqs/sec     74022.64    5566.96   85014.60
    Latency      670.52us   225.00us    13.68ms
    HTTP codes:
      1xx - 0, 2xx - 96533, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3467
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3467
    Throughput:    11.30MB/s
  ```


