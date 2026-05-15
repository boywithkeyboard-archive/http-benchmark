## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73583` | `3620` | `85910` |
| **83%** | [Hyper Express](#hyper-express) | `61053` | `3591` | `71951` |
| **31%** | [Node (Default)](#node-default) | `22949` | `6447` | `62377` |
| **30%** | [Hono](#hono) | `22130` | `6653` | `30157` |
| **29%** | [Fastify](#fastify) | `21561` | `5906` | `37501` |
| **24%** | [Koa](#koa) | `17953` | `8942` | `71944` |
| **10%** | [Carbon](#carbon) | `7329` | `1223` | `9991` |
| **8%** | [Express](#express) | `6141` | `990` | `8069` |


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
    Reqs/sec      8239.14    6705.11   78685.98
    Latency        6.06ms     4.37ms   374.31ms
    HTTP codes:
      1xx - 0, 2xx - 89056, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10944
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10944
    Throughput:     1.67MB/s
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
    Reqs/sec      6036.07     947.04    8042.93
    Latency        8.28ms     3.94ms   376.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     21373.35    6044.95   35905.98
    Latency        2.34ms     2.10ms   191.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     22489.12    7028.73   30955.76
    Latency        2.22ms     2.20ms   195.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.08MB/s
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
    Reqs/sec     60501.55    3880.39   66369.66
    Latency      824.28us    89.33us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.59MB/s
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
    Reqs/sec     18782.98   10490.72   83362.40
    Latency        2.65ms     2.55ms   220.69ms
    HTTP codes:
      1xx - 0, 2xx - 89076, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10924
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10924
    Throughput:     3.79MB/s
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
    Reqs/sec     21236.68    6147.32   70676.36
    Latency        2.35ms     2.10ms   177.89ms
    HTTP codes:
      1xx - 0, 2xx - 96731, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3269
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3269
    Throughput:     4.70MB/s
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
    Reqs/sec     72522.81    3949.53   86708.58
    Latency      686.34us   151.19us     6.10ms
    HTTP codes:
      1xx - 0, 2xx - 96808, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3192
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3192
    Throughput:    11.11MB/s
  ```


