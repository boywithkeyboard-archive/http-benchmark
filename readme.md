## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71347` | `3877` | `79982` |
| **83%** | [Hyper Express](#hyper-express) | `59526` | `4436` | `65696` |
| **34%** | [Node (Default)](#node-default) | `24411` | `6734` | `58407` |
| **31%** | [Fastify](#fastify) | `22071` | `5869` | `37124` |
| **30%** | [Hono](#hono) | `21588` | `6782` | `30986` |
| **25%** | [Koa](#koa) | `18158` | `7830` | `61441` |
| **11%** | [Carbon](#carbon) | `8022` | `1395` | `10266` |
| **9%** | [Express](#express) | `6166` | `1009` | `8198` |


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
    Reqs/sec      8790.83    6488.73   74921.81
    Latency        5.68ms     4.46ms   386.07ms
    HTTP codes:
      1xx - 0, 2xx - 90084, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9916
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9916
    Throughput:     1.80MB/s
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
    Reqs/sec      6336.36    1073.39    8343.64
    Latency        7.89ms     3.76ms   361.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23724.51    7420.68   36617.21
    Latency        2.11ms     2.12ms   190.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     21425.41    6495.30   30567.80
    Latency        2.33ms     2.10ms   187.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     60175.78    3493.20   66598.97
    Latency      829.49us    86.17us     3.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.54MB/s
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
    Reqs/sec     19025.98    8950.02   70047.70
    Latency        2.62ms     2.51ms   221.51ms
    HTTP codes:
      1xx - 0, 2xx - 91342, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8658
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8658
    Throughput:     3.93MB/s
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
    Reqs/sec     24528.22    8008.21   77284.86
    Latency        2.03ms     1.98ms   174.56ms
    HTTP codes:
      1xx - 0, 2xx - 95621, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4379
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4379
    Throughput:     5.37MB/s
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
    Reqs/sec     70868.80    3607.94   77366.70
    Latency      701.57us   163.08us     8.64ms
    HTTP codes:
      1xx - 0, 2xx - 96730, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3270
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3270
    Throughput:    10.86MB/s
  ```


