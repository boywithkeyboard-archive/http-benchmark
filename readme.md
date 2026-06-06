## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79715` | `3424` | `84986` |
| **88%** | [Hyper Express](#hyper-express) | `70247` | `3592` | `74350` |
| **47%** | [Node (Default)](#node-default) | `37236` | `10536` | `57574` |
| **43%** | [Fastify](#fastify) | `34185` | `9547` | `51773` |
| **38%** | [Hono](#hono) | `30101` | `9199` | `46683` |
| **36%** | [Koa](#koa) | `28642` | `12512` | `77578` |
| **12%** | [Carbon](#carbon) | `9470` | `2324` | `13508` |
| **9%** | [Express](#express) | `7304` | `1592` | `10202` |


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
    Reqs/sec     10181.11    8655.29   88795.05
    Latency        4.90ms     4.19ms   361.85ms
    HTTP codes:
      1xx - 0, 2xx - 88297, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11703
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11703
    Throughput:     2.04MB/s
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
    Reqs/sec      8288.18    6785.41   82939.37
    Latency        6.03ms     3.76ms   339.22ms
    HTTP codes:
      1xx - 0, 2xx - 90400, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9600
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9597
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 3
    Throughput:     2.14MB/s
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
    Reqs/sec     33267.37    9425.77   51096.31
    Latency        1.50ms     1.80ms   159.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.55MB/s
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
    Reqs/sec     30601.05    9417.83   47003.88
    Latency        1.63ms     1.96ms   170.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.91MB/s
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
    Reqs/sec     70339.10    3387.67   74848.00
    Latency      709.09us    77.46us     3.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.99MB/s
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
    Reqs/sec     28602.19   11896.22   85529.49
    Latency        1.75ms     2.21ms   192.48ms
    HTTP codes:
      1xx - 0, 2xx - 92544, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7456
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7456
    Throughput:     5.97MB/s
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
    Reqs/sec     35166.00    9767.35   71144.33
    Latency        1.41ms     1.80ms   152.19ms
    HTTP codes:
      1xx - 0, 2xx - 96778, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3222
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3222
    Throughput:     7.81MB/s
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
    Reqs/sec     80741.22    2686.89   84168.32
    Latency      616.87us   142.18us     6.90ms
    HTTP codes:
      1xx - 0, 2xx - 96280, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3720
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3720
    Throughput:    12.31MB/s
  ```


