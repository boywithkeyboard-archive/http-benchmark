## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74721` | `2277` | `79903` |
| **84%** | [Hyper Express](#hyper-express) | `62820` | `2512` | `65157` |
| **35%** | [Node (Default)](#node-default) | `25809` | `8635` | `73362` |
| **32%** | [Fastify](#fastify) | `23816` | `7277` | `36285` |
| **28%** | [Hono](#hono) | `20627` | `5626` | `29811` |
| **25%** | [Koa](#koa) | `18763` | `8260` | `72561` |
| **11%** | [Carbon](#carbon) | `8288` | `1474` | `10320` |
| **8%** | [Express](#express) | `6304` | `999` | `8375` |


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
    Reqs/sec      9230.36    7589.96   78172.25
    Latency        5.40ms     4.35ms   370.98ms
    HTTP codes:
      1xx - 0, 2xx - 88468, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11532
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11532
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
    Reqs/sec      7034.40    6051.04   74470.09
    Latency        7.10ms     3.91ms   359.26ms
    HTTP codes:
      1xx - 0, 2xx - 89725, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10275
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10275
    Throughput:     1.81MB/s
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
    Reqs/sec     25966.40    8723.18   37953.31
    Latency        1.92ms     2.02ms   181.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.89MB/s
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
    Reqs/sec     20693.49    5810.61   30121.95
    Latency        2.41ms     1.95ms   173.86ms
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
    Reqs/sec     61959.96    3481.03   64180.45
    Latency      804.69us    70.86us     2.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     18541.50    9334.08   80261.08
    Latency        2.69ms     2.36ms   209.12ms
    HTTP codes:
      1xx - 0, 2xx - 90791, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9209
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9209
    Throughput:     3.80MB/s
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
    Reqs/sec     26678.25    9254.83   77955.59
    Latency        1.87ms     1.87ms   161.34ms
    HTTP codes:
      1xx - 0, 2xx - 96745, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3255
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3255
    Throughput:     5.91MB/s
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
    Reqs/sec     75801.93    2432.05   82113.53
    Latency      656.69us   165.12us    10.82ms
    HTTP codes:
      1xx - 0, 2xx - 96505, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3495
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3495
    Throughput:    11.58MB/s
  ```


