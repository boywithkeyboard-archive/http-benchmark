## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71304` | `4268` | `87270` |
| **83%** | [Hyper Express](#hyper-express) | `59081` | `3707` | `63679` |
| **34%** | [Node (Default)](#node-default) | `24283` | `7949` | `73437` |
| **33%** | [Fastify](#fastify) | `23335` | `7295` | `33793` |
| **27%** | [Hono](#hono) | `19193` | `5584` | `28547` |
| **24%** | [Koa](#koa) | `17228` | `7924` | `70411` |
| **11%** | [Carbon](#carbon) | `7761` | `1234` | `10937` |
| **9%** | [Express](#express) | `6341` | `1010` | `8232` |


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
    Reqs/sec      8590.11    6513.11   71686.64
    Latency        5.81ms     4.57ms   383.08ms
    HTTP codes:
      1xx - 0, 2xx - 89311, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10689
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10689
    Throughput:     1.74MB/s
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
    Reqs/sec      6361.25    1045.46    8208.54
    Latency        7.86ms     3.71ms   358.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     23459.84    7153.36   33700.45
    Latency        2.13ms     2.05ms   179.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.32MB/s
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
    Reqs/sec     19725.73    5914.42   28571.19
    Latency        2.53ms     2.24ms   198.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.46MB/s
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
    Reqs/sec     58764.09    3988.32   64767.76
    Latency      849.61us    98.42us     4.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.34MB/s
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
    Reqs/sec     18216.25    7393.46   60575.41
    Latency        2.73ms     2.37ms   210.16ms
    HTTP codes:
      1xx - 0, 2xx - 93344, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6656
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6656
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
    Reqs/sec     24958.83    8817.54   61177.29
    Latency        2.00ms     1.95ms   169.13ms
    HTTP codes:
      1xx - 0, 2xx - 97217, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2783
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2783
    Throughput:     5.56MB/s
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
    Reqs/sec     68915.17    3399.56   77362.52
    Latency      722.76us   196.79us    10.00ms
    HTTP codes:
      1xx - 0, 2xx - 96700, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3300
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3300
    Throughput:    10.55MB/s
  ```


