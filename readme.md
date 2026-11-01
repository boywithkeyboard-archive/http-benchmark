## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75554` | `4823` | `88053` |
| **82%** | [Hyper Express](#hyper-express) | `61806` | `3616` | `75106` |
| **35%** | [Node (Default)](#node-default) | `26698` | `8487` | `69077` |
| **32%** | [Fastify](#fastify) | `24521` | `6902` | `34779` |
| **27%** | [Hono](#hono) | `20751` | `5561` | `28887` |
| **24%** | [Koa](#koa) | `17778` | `7623` | `70977` |
| **10%** | [Carbon](#carbon) | `7802` | `1238` | `10482` |
| **9%** | [Express](#express) | `6515` | `1048` | `8410` |


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
    Reqs/sec      8788.35    6713.23   76699.24
    Latency        5.67ms     4.51ms   381.67ms
    HTTP codes:
      1xx - 0, 2xx - 89625, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10375
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10375
    Throughput:     1.79MB/s
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
    Reqs/sec      6571.06    1055.92    8388.74
    Latency        7.61ms     3.72ms   360.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     25133.58    7152.32   33690.09
    Latency        1.99ms     2.00ms   178.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.70MB/s
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
    Reqs/sec     19755.94    5587.44   28679.96
    Latency        2.53ms     2.27ms   199.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.46MB/s
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
    Reqs/sec     61620.14    3778.13   68301.77
    Latency      810.02us    85.40us     3.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.74MB/s
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
    Reqs/sec     19290.43    9767.81   78990.00
    Latency        2.58ms     2.40ms   209.85ms
    HTTP codes:
      1xx - 0, 2xx - 89581, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10419
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10419
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
    Reqs/sec     26692.47    8970.44   73990.58
    Latency        1.87ms     1.93ms   163.17ms
    HTTP codes:
      1xx - 0, 2xx - 96508, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3492
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3492
    Throughput:     5.90MB/s
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
    Reqs/sec     73123.77    7689.27   88147.03
    Latency      680.58us   266.12us    14.36ms
    HTTP codes:
      1xx - 0, 2xx - 96111, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3889
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3889
    Throughput:    11.12MB/s
  ```


