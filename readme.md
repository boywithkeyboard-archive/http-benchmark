## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `100721` | `3716` | `112182` |
| **87%** | [Hyper Express](#hyper-express) | `87699` | `3843` | `91421` |
| **46%** | [Node (Default)](#node-default) | `46079` | `15096` | `75615` |
| **43%** | [Fastify](#fastify) | `43252` | `13500` | `65382` |
| **38%** | [Hono](#hono) | `38272` | `12860` | `59674` |
| **31%** | [Koa](#koa) | `30855` | `12794` | `96384` |
| **12%** | [Carbon](#carbon) | `11914` | `2791` | `17172` |
| **9%** | [Express](#express) | `9263` | `1839` | `13207` |


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
    Reqs/sec     12113.74    7933.39   98262.51
    Latency        4.11ms     3.40ms   293.13ms
    HTTP codes:
      1xx - 0, 2xx - 92062, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7938
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7938
    Throughput:     2.54MB/s
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
    Reqs/sec      9363.38    1840.79   13396.28
    Latency        5.34ms     2.77ms   264.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     2.68MB/s
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
    Reqs/sec     49711.23   17163.25   67173.61
    Latency        1.01ms     1.48ms   129.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    11.27MB/s
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
    Reqs/sec     35514.13   11842.61   59301.10
    Latency        1.41ms     1.51ms   132.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.03MB/s
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
    Reqs/sec     85906.73    4116.21   88857.81
    Latency      580.14us    56.45us     2.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.21MB/s
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
    Reqs/sec     32084.40   15712.91  112086.37
    Latency        1.55ms     1.74ms   147.71ms
    HTTP codes:
      1xx - 0, 2xx - 88271, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11729
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11729
    Throughput:     6.41MB/s
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
    Reqs/sec     51440.25   17365.20   88884.31
    Latency        0.97ms     1.32ms   108.47ms
    HTTP codes:
      1xx - 0, 2xx - 96394, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3606
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3606
    Throughput:    11.34MB/s
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
    Reqs/sec    101434.23    3059.56  109498.05
    Latency      490.66us   133.70us     8.38ms
    HTTP codes:
      1xx - 0, 2xx - 95180, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4820
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4820
    Throughput:    15.29MB/s
  ```


