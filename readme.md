## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73695` | `5755` | `93053` |
| **86%** | [Hyper Express](#hyper-express) | `63324` | `4005` | `69801` |
| **34%** | [Node (Default)](#node-default) | `24931` | `6618` | `51866` |
| **31%** | [Fastify](#fastify) | `23188` | `6992` | `35329` |
| **28%** | [Hono](#hono) | `20383` | `5563` | `28997` |
| **25%** | [Koa](#koa) | `18749` | `6715` | `59597` |
| **11%** | [Carbon](#carbon) | `8302` | `1428` | `10465` |
| **9%** | [Express](#express) | `6583` | `1020` | `8538` |


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
    Reqs/sec      8731.96    4263.75   50506.17
    Latency        5.71ms     4.17ms   362.83ms
    HTTP codes:
      1xx - 0, 2xx - 93545, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6455
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6455
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
    Reqs/sec      6638.63    1077.75    8521.68
    Latency        7.53ms     3.51ms   336.22ms
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
    Reqs/sec     23200.49    7004.48   36212.45
    Latency        2.15ms     1.94ms   176.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.26MB/s
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
    Reqs/sec     21216.17    6074.45   29764.28
    Latency        2.36ms     1.91ms   174.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     62527.93    5238.28   80046.37
    Latency      797.21us   123.41us     6.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18761.52    7265.31   57656.93
    Latency        2.66ms     2.33ms   208.07ms
    HTTP codes:
      1xx - 0, 2xx - 92198, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7802
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7802
    Throughput:     3.91MB/s
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
    Reqs/sec     26544.75    7426.80   51871.95
    Latency        1.88ms     1.84ms   157.68ms
    HTTP codes:
      1xx - 0, 2xx - 97506, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2494
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2494
    Throughput:     5.93MB/s
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
    Reqs/sec     74400.67    6033.75   82755.49
    Latency      670.53us   179.95us    10.36ms
    HTTP codes:
      1xx - 0, 2xx - 98067, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1933
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1933
    Throughput:    11.53MB/s
  ```


