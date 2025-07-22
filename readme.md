## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73802` | `2324` | `77684` |
| **83%** | [Hyper Express](#hyper-express) | `61318` | `3713` | `64458` |
| **34%** | [Node (Default)](#node-default) | `25378` | `8009` | `61916` |
| **32%** | [Fastify](#fastify) | `23481` | `6994` | `35041` |
| **27%** | [Hono](#hono) | `20166` | `5940` | `29483` |
| **26%** | [Koa](#koa) | `19046` | `9470` | `78616` |
| **11%** | [Carbon](#carbon) | `8170` | `1495` | `10274` |
| **8%** | [Express](#express) | `6259` | `1048` | `8329` |


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
    Reqs/sec      9033.87    7410.23   78844.73
    Latency        5.52ms     4.43ms   383.63ms
    HTTP codes:
      1xx - 0, 2xx - 88249, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11751
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11751
    Throughput:     1.81MB/s
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
    Reqs/sec      6976.59    6604.49   78892.27
    Latency        7.16ms     4.16ms   380.44ms
    HTTP codes:
      1xx - 0, 2xx - 88139, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11861
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11861
    Throughput:     1.76MB/s
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
    Reqs/sec     24523.22    7508.85   34184.04
    Latency        2.04ms     2.01ms   182.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     21068.24    6298.04   29425.59
    Latency        2.37ms     2.07ms   187.40ms
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
    Reqs/sec     61548.25    3477.28   65516.28
    Latency      809.31us    69.79us     2.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     19002.45    9432.76   81738.25
    Latency        2.62ms     2.47ms   216.30ms
    HTTP codes:
      1xx - 0, 2xx - 90490, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9510
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9510
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
    Reqs/sec     25204.32    7914.06   78096.01
    Latency        1.98ms     2.00ms   171.90ms
    HTTP codes:
      1xx - 0, 2xx - 96396, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3604
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3604
    Throughput:     5.57MB/s
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
    Reqs/sec     73746.99    2964.49   79508.86
    Latency      675.65us   164.08us     7.91ms
    HTTP codes:
      1xx - 0, 2xx - 97347, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2653
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2653
    Throughput:    11.36MB/s
  ```


