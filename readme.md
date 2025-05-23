## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74854` | `4923` | `83822` |
| **83%** | [Hyper Express](#hyper-express) | `61967` | `3758` | `66463` |
| **38%** | [Node (Default)](#node-default) | `28089` | `8720` | `55888` |
| **32%** | [Fastify](#fastify) | `24221` | `7002` | `36624` |
| **27%** | [Hono](#hono) | `20511` | `6232` | `30774` |
| **25%** | [Koa](#koa) | `18916` | `7291` | `59414` |
| **11%** | [Carbon](#carbon) | `8437` | `1431` | `10433` |
| **9%** | [Express](#express) | `6678` | `1132` | `8491` |


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
    Reqs/sec      9062.03    4953.62   65334.67
    Latency        5.51ms     4.27ms   369.06ms
    HTTP codes:
      1xx - 0, 2xx - 92995, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7005
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7005
    Throughput:     1.91MB/s
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
    Reqs/sec      7160.07    4534.37   60115.91
    Latency        6.97ms     3.88ms   354.66ms
    HTTP codes:
      1xx - 0, 2xx - 92355, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7645
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7645
    Throughput:     1.89MB/s
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
    Reqs/sec     25219.79    7593.48   37255.15
    Latency        1.98ms     1.97ms   174.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.71MB/s
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
    Reqs/sec     21840.67    6263.89   30928.19
    Latency        2.29ms     2.02ms   182.98ms
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
    Reqs/sec     61370.47    2468.14   65247.43
    Latency      812.22us    73.52us     3.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     18319.83    6428.25   54449.09
    Latency        2.72ms     2.41ms   206.56ms
    HTTP codes:
      1xx - 0, 2xx - 94802, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5198
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5198
    Throughput:     3.93MB/s
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
    Reqs/sec     26911.06    7887.49   51033.89
    Latency        1.85ms     1.92ms   167.08ms
    HTTP codes:
      1xx - 0, 2xx - 97837, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2163
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2163
    Throughput:     6.03MB/s
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
    Reqs/sec     74195.73    4325.02   89314.89
    Latency      670.22us   163.88us    10.68ms
    HTTP codes:
      1xx - 0, 2xx - 96494, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3506
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3506
    Throughput:    11.33MB/s
  ```


