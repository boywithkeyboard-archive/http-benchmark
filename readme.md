## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73793` | `5864` | `82488` |
| **84%** | [Hyper Express](#hyper-express) | `62081` | `2764` | `65044` |
| **35%** | [Node (Default)](#node-default) | `25759` | `7696` | `55357` |
| **34%** | [Fastify](#fastify) | `24834` | `7576` | `36582` |
| **28%** | [Hono](#hono) | `20696` | `5891` | `30353` |
| **25%** | [Koa](#koa) | `18192` | `6226` | `53377` |
| **11%** | [Carbon](#carbon) | `8288` | `1413` | `10708` |
| **9%** | [Express](#express) | `6563` | `1022` | `8606` |


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
    Reqs/sec      8756.48    4522.90   64563.63
    Latency        5.70ms     4.15ms   355.98ms
    HTTP codes:
      1xx - 0, 2xx - 93387, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6613
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6613
    Throughput:     1.86MB/s
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
    Reqs/sec      6497.83    1025.17    8462.37
    Latency        7.69ms     3.62ms   344.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24124.57    7381.24   36309.30
    Latency        2.07ms     1.96ms   178.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     20868.49    5919.59   30015.71
    Latency        2.39ms     1.93ms   176.46ms
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
    Reqs/sec     62233.99    3539.21   71841.71
    Latency      800.87us    81.71us     3.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18014.74    6392.67   63034.77
    Latency        2.76ms     2.43ms   211.08ms
    HTTP codes:
      1xx - 0, 2xx - 93953, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6047
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6047
    Throughput:     3.84MB/s
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
    Reqs/sec     24745.06    6954.69   55758.48
    Latency        2.01ms     1.60ms   143.58ms
    HTTP codes:
      1xx - 0, 2xx - 96538, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3462
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3462
    Throughput:     5.49MB/s
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
    Reqs/sec     73459.25    4793.31   82393.08
    Latency      679.19us   159.47us    12.50ms
    HTTP codes:
      1xx - 0, 2xx - 97518, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2482
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2482
    Throughput:    11.31MB/s
  ```


