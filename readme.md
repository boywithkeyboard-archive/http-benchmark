## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73713` | `5671` | `78829` |
| **83%** | [Hyper Express](#hyper-express) | `60997` | `3257` | `73677` |
| **36%** | [Node (Default)](#node-default) | `26184` | `7700` | `48077` |
| **34%** | [Fastify](#fastify) | `24849` | `7246` | `35592` |
| **29%** | [Hono](#hono) | `21679` | `5989` | `29775` |
| **25%** | [Koa](#koa) | `18149` | `6512` | `56934` |
| **11%** | [Carbon](#carbon) | `8264` | `1448` | `10563` |
| **9%** | [Express](#express) | `6542` | `1070` | `8408` |


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
    Reqs/sec      8715.00    4228.44   59268.20
    Latency        5.73ms     4.11ms   359.43ms
    HTTP codes:
      1xx - 0, 2xx - 94475, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5525
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5525
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
    Reqs/sec      6567.99    1061.56    8506.61
    Latency        7.61ms     3.52ms   337.01ms
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
    Reqs/sec     25171.45    8121.86   36862.34
    Latency        1.98ms     2.00ms   179.69ms
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
    Reqs/sec     21079.60    5909.48   29533.38
    Latency        2.37ms     2.01ms   179.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     63048.56    3822.59   79484.41
    Latency      791.21us    65.90us     3.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     18197.69    6607.55   59372.15
    Latency        2.74ms     2.35ms   204.78ms
    HTTP codes:
      1xx - 0, 2xx - 94452, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5548
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5548
    Throughput:     3.89MB/s
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
    Reqs/sec     25178.72    6814.10   48122.02
    Latency        1.98ms     1.86ms   156.60ms
    HTTP codes:
      1xx - 0, 2xx - 97654, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2346
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2346
    Throughput:     5.63MB/s
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
    Reqs/sec     74135.10    5125.13   81205.87
    Latency      672.01us   217.29us    10.10ms
    HTTP codes:
      1xx - 0, 2xx - 97371, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2629
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2629
    Throughput:    11.42MB/s
  ```


