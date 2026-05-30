## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71171` | `5014` | `93553` |
| **83%** | [Hyper Express](#hyper-express) | `59331` | `3821` | `65839` |
| **29%** | [Node (Default)](#node-default) | `20840` | `6679` | `68672` |
| **29%** | [Hono](#hono) | `20582` | `6106` | `29609` |
| **27%** | [Fastify](#fastify) | `19397` | `5037` | `35842` |
| **27%** | [Koa](#koa) | `19212` | `8665` | `70261` |
| **10%** | [Carbon](#carbon) | `7377` | `1198` | `10419` |
| **9%** | [Express](#express) | `6102` | `1052` | `8245` |


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
    Reqs/sec      8155.95    6895.01   77496.43
    Latency        6.12ms     4.50ms   385.65ms
    HTTP codes:
      1xx - 0, 2xx - 87972, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12028
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12028
    Throughput:     1.63MB/s
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
    Reqs/sec      6092.66    1029.29    8219.88
    Latency        8.20ms     3.63ms   352.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     19738.35    4706.30   34533.10
    Latency        2.53ms     2.01ms   181.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.48MB/s
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
    Reqs/sec     20761.41    6181.53   29946.66
    Latency        2.41ms     2.15ms   190.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     58581.81    3151.68   64629.58
    Latency        0.85ms    93.66us     3.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.32MB/s
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
    Reqs/sec     18911.96    8197.47   62338.75
    Latency        2.64ms     2.47ms   214.35ms
    HTTP codes:
      1xx - 0, 2xx - 93085, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6915
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6915
    Throughput:     3.98MB/s
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
    Reqs/sec     20756.51    5285.72   59774.96
    Latency        2.40ms     1.93ms   166.80ms
    HTTP codes:
      1xx - 0, 2xx - 97676, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2324
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2324
    Throughput:     4.64MB/s
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
    Reqs/sec     69546.76    3209.38   78648.99
    Latency      716.24us   179.20us     7.94ms
    HTTP codes:
      1xx - 0, 2xx - 95226, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4774
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4774
    Throughput:    10.48MB/s
  ```


