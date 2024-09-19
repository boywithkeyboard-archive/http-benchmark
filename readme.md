## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72905` | `5820` | `87074` |
| **83%** | [Hyper Express](#hyper-express) | `60620` | `3974` | `70776` |
| **34%** | [Node (Default)](#node-default) | `24773` | `7052` | `54209` |
| **31%** | [Fastify](#fastify) | `22870` | `6687` | `34727` |
| **28%** | [Hono](#hono) | `20083` | `5021` | `28282` |
| **24%** | [Koa](#koa) | `17475` | `6309` | `54338` |
| **11%** | [Carbon](#carbon) | `8015` | `1372` | `10203` |
| **8%** | [Express](#express) | `6164` | `981` | `8191` |


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
    Reqs/sec      8609.47    4978.98   56123.98
    Latency        5.80ms     4.45ms   384.11ms
    HTTP codes:
      1xx - 0, 2xx - 92263, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7737
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7737
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
    Reqs/sec      6224.57    1019.02    8295.35
    Latency        8.03ms     3.85ms   365.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     24076.60    7147.89   36773.78
    Latency        2.07ms     2.13ms   190.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     21550.23    6151.71   30666.66
    Latency        2.32ms     2.02ms   182.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     61052.59    3010.02   65781.44
    Latency      816.68us    74.68us     2.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.67MB/s
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
    Reqs/sec     17798.85    5960.01   53068.73
    Latency        2.80ms     2.34ms   205.31ms
    HTTP codes:
      1xx - 0, 2xx - 94892, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5108
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5108
    Throughput:     3.82MB/s
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
    Reqs/sec     25866.39    7618.35   55589.93
    Latency        1.93ms     1.95ms   168.66ms
    HTTP codes:
      1xx - 0, 2xx - 96753, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3247
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3247
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
    Reqs/sec     72213.18    4738.83   83773.02
    Latency      689.75us   158.05us     7.00ms
    HTTP codes:
      1xx - 0, 2xx - 97206, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2794
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2794
    Throughput:    11.11MB/s
  ```


