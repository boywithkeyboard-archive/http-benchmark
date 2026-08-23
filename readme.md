## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `67531` | `3244` | `72744` |
| **87%** | [Hyper Express](#hyper-express) | `58845` | `2632` | `62633` |
| **47%** | [Node (Default)](#node-default) | `31780` | `9367` | `66786` |
| **41%** | [Fastify](#fastify) | `27928` | `7362` | `45518` |
| **41%** | [Hono](#hono) | `27457` | `9204` | `40373` |
| **40%** | [Koa](#koa) | `27029` | `11070` | `64200` |
| **13%** | [Carbon](#carbon) | `8479` | `2196` | `12279` |
| **10%** | [Express](#express) | `6743` | `1526` | `9446` |


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
    Reqs/sec      9347.96    6114.33   65621.56
    Latency        5.34ms     5.20ms   443.32ms
    HTTP codes:
      1xx - 0, 2xx - 91717, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8283
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8283
    Throughput:     1.95MB/s
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
    Reqs/sec      6591.59    1483.97    9744.65
    Latency        7.58ms     3.94ms   372.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     29112.92    8315.58   44691.11
    Latency        1.72ms     2.27ms   200.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.61MB/s
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
    Reqs/sec     26791.75    8205.03   41431.31
    Latency        1.86ms     2.50ms   215.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.05MB/s
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
    Reqs/sec     59169.15    2589.46   61857.28
    Latency      843.35us    86.18us     2.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.40MB/s
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
    Reqs/sec     26401.59   10308.22   65530.82
    Latency        1.89ms     2.45ms   208.61ms
    HTTP codes:
      1xx - 0, 2xx - 92990, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7010
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7010
    Throughput:     5.56MB/s
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
    Reqs/sec     31518.93    9082.13   72108.85
    Latency        1.58ms     2.06ms   175.62ms
    HTTP codes:
      1xx - 0, 2xx - 96389, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3611
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3611
    Throughput:     6.95MB/s
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
    Reqs/sec     67645.24    2034.19   71142.08
    Latency      736.58us   188.17us     9.64ms
    HTTP codes:
      1xx - 0, 2xx - 95010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4990
    Throughput:    10.17MB/s
  ```


