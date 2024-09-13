## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73667` | `7420` | `90792` |
| **85%** | [Hyper Express](#hyper-express) | `62462` | `3914` | `68736` |
| **36%** | [Node (Default)](#node-default) | `26440` | `7438` | `45399` |
| **34%** | [Fastify](#fastify) | `25303` | `8022` | `38035` |
| **29%** | [Hono](#hono) | `21557` | `5906` | `31017` |
| **23%** | [Koa](#koa) | `17024` | `4964` | `37588` |
| **11%** | [Carbon](#carbon) | `8357` | `1407` | `10639` |
| **9%** | [Express](#express) | `6557` | `1053` | `8490` |


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
    Reqs/sec      8867.22    4559.07   58056.64
    Latency        5.61ms     4.20ms   366.64ms
    HTTP codes:
      1xx - 0, 2xx - 93320, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6680
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6680
    Throughput:     1.89MB/s
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
    Reqs/sec      6645.40    1085.11    8654.62
    Latency        7.52ms     3.52ms   336.63ms
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
    Reqs/sec     24768.75    7601.30   37721.05
    Latency        2.02ms     1.98ms   176.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.61MB/s
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
    Reqs/sec     21644.35    6181.91   30768.84
    Latency        2.31ms     1.98ms   178.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.88MB/s
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
    Reqs/sec     63168.37    2733.08   66881.79
    Latency      789.62us    62.56us     3.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     17788.77    5879.76   45053.03
    Latency        2.80ms     2.33ms   202.90ms
    HTTP codes:
      1xx - 0, 2xx - 95615, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4385
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4385
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
    Reqs/sec     25705.49    7750.69   52970.57
    Latency        1.94ms     1.88ms   158.47ms
    HTTP codes:
      1xx - 0, 2xx - 97046, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2954
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2954
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
    Reqs/sec     73943.70    5892.89   93044.65
    Latency      674.04us   189.27us    10.04ms
    HTTP codes:
      1xx - 0, 2xx - 97780, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2220
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2220
    Throughput:    11.43MB/s
  ```


