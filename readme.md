## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73605` | `3901` | `83188` |
| **81%** | [Hyper Express](#hyper-express) | `59653` | `3330` | `64786` |
| **31%** | [Fastify](#fastify) | `22870` | `6842` | `39543` |
| **30%** | [Node (Default)](#node-default) | `22431` | `5704` | `58188` |
| **30%** | [Hono](#hono) | `22244` | `6960` | `32988` |
| **24%** | [Koa](#koa) | `17993` | `7831` | `67143` |
| **11%** | [Carbon](#carbon) | `7988` | `1528` | `10443` |
| **9%** | [Express](#express) | `6389` | `1202` | `8823` |


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
    Reqs/sec      8990.33    5932.99   68044.23
    Latency        5.55ms     4.27ms   364.37ms
    HTTP codes:
      1xx - 0, 2xx - 91449, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8551
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8551
    Throughput:     1.87MB/s
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
    Reqs/sec      6487.72    1244.74    8910.30
    Latency        7.70ms     3.81ms   363.73ms
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
    Reqs/sec     22094.29    6452.19   39070.81
    Latency        2.26ms     2.16ms   194.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.01MB/s
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
    Reqs/sec     21807.73    7003.55   32800.80
    Latency        2.29ms     2.03ms   183.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.93MB/s
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
    Reqs/sec     60137.91    3960.64   67621.03
    Latency      829.58us    82.09us     2.86ms
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
    Reqs/sec     18265.86    8826.20   72035.93
    Latency        2.73ms     2.39ms   211.42ms
    HTTP codes:
      1xx - 0, 2xx - 92212, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7788
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7788
    Throughput:     3.81MB/s
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
    Reqs/sec     22518.02    6205.83   76252.71
    Latency        2.22ms     2.03ms   174.30ms
    HTTP codes:
      1xx - 0, 2xx - 96682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3318
    Throughput:     4.99MB/s
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
    Reqs/sec     72305.72    3471.80   82507.10
    Latency      688.30us   156.40us     7.72ms
    HTTP codes:
      1xx - 0, 2xx - 96512, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3488
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3488
    Throughput:    11.05MB/s
  ```


