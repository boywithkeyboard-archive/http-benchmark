## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71521` | `4958` | `85720` |
| **85%** | [Hyper Express](#hyper-express) | `60668` | `4255` | `68386` |
| **30%** | [Hono](#hono) | `21513` | `6534` | `30698` |
| **30%** | [Fastify](#fastify) | `21397` | `6113` | `36340` |
| **29%** | [Node (Default)](#node-default) | `20814` | `6567` | `73005` |
| **28%** | [Koa](#koa) | `20139` | `9420` | `68772` |
| **10%** | [Carbon](#carbon) | `7353` | `1248` | `10424` |
| **8%** | [Express](#express) | `5970` | `994` | `8186` |


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
    Reqs/sec      7982.13    5240.17   65636.85
    Latency        6.25ms     4.44ms   382.61ms
    HTTP codes:
      1xx - 0, 2xx - 92107, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7893
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7893
    Throughput:     1.67MB/s
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
    Reqs/sec      6059.34    1071.59    8507.86
    Latency        8.25ms     3.87ms   369.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     21224.28    6418.04   35040.16
    Latency        2.35ms     2.18ms   196.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     21804.70    6915.18   31411.77
    Latency        2.29ms     2.12ms   188.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.92MB/s
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
    Reqs/sec     60363.73    4000.30   68991.19
    Latency      826.08us    98.05us     3.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.57MB/s
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
    Reqs/sec     19419.60    8913.66   74699.16
    Latency        2.57ms     2.53ms   222.08ms
    HTTP codes:
      1xx - 0, 2xx - 92662, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7338
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7338
    Throughput:     4.07MB/s
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
    Reqs/sec     19859.32    5125.15   68596.04
    Latency        2.51ms     2.05ms   176.78ms
    HTTP codes:
      1xx - 0, 2xx - 96792, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3208
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3208
    Throughput:     4.40MB/s
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
    Reqs/sec     71881.18    3534.04   84816.74
    Latency      691.79us   190.18us    12.17ms
    HTTP codes:
      1xx - 0, 2xx - 94046, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5954
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5954
    Throughput:    10.71MB/s
  ```


