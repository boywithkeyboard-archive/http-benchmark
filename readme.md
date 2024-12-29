## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74383` | `5618` | `85072` |
| **85%** | [Hyper Express](#hyper-express) | `63068` | `3669` | `66407` |
| **34%** | [Node (Default)](#node-default) | `25558` | `7058` | `41322` |
| **34%** | [Fastify](#fastify) | `25051` | `7627` | `36910` |
| **28%** | [Hono](#hono) | `21192` | `5707` | `29962` |
| **25%** | [Koa](#koa) | `18332` | `6204` | `47377` |
| **11%** | [Carbon](#carbon) | `8338` | `1444` | `10523` |
| **9%** | [Express](#express) | `6586` | `1073` | `8553` |


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
    Reqs/sec      8538.34    2868.36   40949.40
    Latency        5.84ms     4.15ms   361.06ms
    HTTP codes:
      1xx - 0, 2xx - 96457, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3543
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3543
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
    Reqs/sec      6641.56    1054.55    8591.27
    Latency        7.52ms     3.52ms   335.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.90MB/s
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
    Reqs/sec     23702.76    6559.58   35477.78
    Latency        2.11ms     1.90ms   172.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     20903.50    5916.37   30579.78
    Latency        2.39ms     1.89ms   172.25ms
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
    Reqs/sec     63406.91    2950.91   68841.81
    Latency      786.40us    65.08us     3.30ms
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
    Reqs/sec     18584.02    6474.61   48998.39
    Latency        2.69ms     2.24ms   194.65ms
    HTTP codes:
      1xx - 0, 2xx - 94666, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5334
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5334
    Throughput:     3.98MB/s
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
    Reqs/sec     25777.72    7810.34   56616.84
    Latency        1.94ms     1.87ms   160.22ms
    HTTP codes:
      1xx - 0, 2xx - 97006, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2994
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2994
    Throughput:     5.72MB/s
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
    Reqs/sec     74343.04    5454.07   80075.06
    Latency      669.49us   210.32us    10.65ms
    HTTP codes:
      1xx - 0, 2xx - 97933, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2067
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2067
    Throughput:    11.53MB/s
  ```


