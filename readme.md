## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68712` | `3855` | `77206` |
| **84%** | [Hyper Express](#hyper-express) | `57595` | `4532` | `64585` |
| **31%** | [Hono](#hono) | `21508` | `6823` | `31287` |
| **30%** | [Node (Default)](#node-default) | `20894` | `5381` | `62912` |
| **29%** | [Fastify](#fastify) | `19797` | `5048` | `36460` |
| **29%** | [Koa](#koa) | `19791` | `8906` | `67426` |
| **11%** | [Carbon](#carbon) | `7279` | `1214` | `10188` |
| **9%** | [Express](#express) | `6182` | `1130` | `8221` |


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
    Reqs/sec      8003.93    5027.31   62438.09
    Latency        6.24ms     4.67ms   390.35ms
    HTTP codes:
      1xx - 0, 2xx - 93014, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6986
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6986
    Throughput:     1.69MB/s
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
    Reqs/sec      6254.13    1133.75    8247.75
    Latency        7.99ms     3.93ms   376.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     20196.72    5254.76   35340.05
    Latency        2.47ms     2.04ms   182.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     20779.89    5803.29   30197.35
    Latency        2.40ms     2.40ms   206.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     58530.55    3024.41   63674.33
    Latency        0.85ms    91.17us     2.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.31MB/s
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
    Reqs/sec     18722.89    7475.99   58912.41
    Latency        2.67ms     2.39ms   207.41ms
    HTTP codes:
      1xx - 0, 2xx - 94534, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5466
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5466
    Throughput:     4.00MB/s
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
    Reqs/sec     20240.66    5687.02   60552.96
    Latency        2.47ms     1.98ms   172.64ms
    HTTP codes:
      1xx - 0, 2xx - 96423, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3577
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3577
    Throughput:     4.47MB/s
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
    Reqs/sec     68335.72    5111.10   81719.95
    Latency      726.91us   208.17us    11.31ms
    HTTP codes:
      1xx - 0, 2xx - 96574, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3426
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3426
    Throughput:    10.46MB/s
  ```


