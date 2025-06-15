## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73277` | `5021` | `81797` |
| **86%** | [Hyper Express](#hyper-express) | `62763` | `3442` | `66246` |
| **35%** | [Node (Default)](#node-default) | `25382` | `7819` | `48778` |
| **32%** | [Fastify](#fastify) | `23376` | `7352` | `35531` |
| **29%** | [Hono](#hono) | `21237` | `6074` | `30820` |
| **26%** | [Koa](#koa) | `19278` | `6716` | `54802` |
| **11%** | [Carbon](#carbon) | `8293` | `1437` | `10353` |
| **9%** | [Express](#express) | `6546` | `1086` | `8532` |


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
    Reqs/sec      8915.33    4761.93   55671.44
    Latency        5.59ms     4.28ms   368.34ms
    HTTP codes:
      1xx - 0, 2xx - 92705, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7295
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7295
    Throughput:     1.88MB/s
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
    Reqs/sec      7019.34    4110.26   57310.56
    Latency        7.12ms     3.86ms   352.09ms
    HTTP codes:
      1xx - 0, 2xx - 93011, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6989
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6989
    Throughput:     1.87MB/s
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
    Reqs/sec     24772.09    7594.69   36410.10
    Latency        2.02ms     1.93ms   172.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     20755.82    6189.02   30667.22
    Latency        2.41ms     2.07ms   188.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     62799.54    3705.17   70836.23
    Latency      793.92us    82.25us     3.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     19333.40    7497.41   56260.01
    Latency        2.58ms     2.33ms   206.45ms
    HTTP codes:
      1xx - 0, 2xx - 92699, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7301
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7301
    Throughput:     4.05MB/s
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
    Reqs/sec     25557.07    7432.00   56500.20
    Latency        1.95ms     1.93ms   162.83ms
    HTTP codes:
      1xx - 0, 2xx - 97009, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2991
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2991
    Throughput:     5.68MB/s
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
    Reqs/sec     73145.88    4746.53   77720.45
    Latency      681.32us   152.40us     5.66ms
    HTTP codes:
      1xx - 0, 2xx - 97600, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2400
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2400
    Throughput:    11.29MB/s
  ```


