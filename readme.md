## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74733` | `5697` | `83961` |
| **83%** | [Hyper Express](#hyper-express) | `62110` | `4359` | `68289` |
| **37%** | [Node (Default)](#node-default) | `27576` | `7926` | `48665` |
| **33%** | [Fastify](#fastify) | `24474` | `7327` | `37346` |
| **28%** | [Hono](#hono) | `21032` | `5565` | `29708` |
| **26%** | [Koa](#koa) | `19711` | `6713` | `52964` |
| **11%** | [Carbon](#carbon) | `8338` | `1372` | `10407` |
| **9%** | [Express](#express) | `6566` | `1043` | `8570` |


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
    Reqs/sec      8798.24    4286.71   52044.79
    Latency        5.67ms     4.31ms   374.29ms
    HTTP codes:
      1xx - 0, 2xx - 94280, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5720
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5720
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
    Reqs/sec      7166.38    5138.15   63406.64
    Latency        6.97ms     3.82ms   346.54ms
    HTTP codes:
      1xx - 0, 2xx - 90784, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9216
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9216
    Throughput:     1.86MB/s
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
    Reqs/sec     24761.04    7320.49   36907.66
    Latency        2.02ms     1.94ms   175.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     21803.27    6079.68   29940.16
    Latency        2.29ms     1.98ms   177.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.93MB/s
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
    Reqs/sec     63881.00    3394.16   69664.67
    Latency      779.06us    68.05us     3.21ms
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
    Reqs/sec     19153.75    7681.75   62355.22
    Latency        2.61ms     2.38ms   208.39ms
    HTTP codes:
      1xx - 0, 2xx - 92277, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7723
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7723
    Throughput:     3.99MB/s
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
    Reqs/sec     27652.53    7336.92   55549.88
    Latency        1.81ms     1.79ms   156.13ms
    HTTP codes:
      1xx - 0, 2xx - 97545, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2455
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2455
    Throughput:     6.16MB/s
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
    Reqs/sec     74593.63    4864.43   83381.55
    Latency      667.79us   208.28us    11.28ms
    HTTP codes:
      1xx - 0, 2xx - 96000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4000
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4000
    Throughput:    11.32MB/s
  ```


