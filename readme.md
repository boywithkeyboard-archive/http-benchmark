## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73800` | `5525` | `88588` |
| **85%** | [Hyper Express](#hyper-express) | `62751` | `3425` | `71737` |
| **35%** | [Node (Default)](#node-default) | `25939` | `7936` | `55452` |
| **32%** | [Fastify](#fastify) | `23766` | `7214` | `35463` |
| **29%** | [Hono](#hono) | `21189` | `6260` | `30301` |
| **24%** | [Koa](#koa) | `17646` | `6025` | `51069` |
| **11%** | [Carbon](#carbon) | `8258` | `1467` | `10448` |
| **8%** | [Express](#express) | `6243` | `956` | `8321` |


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
    Reqs/sec      8752.01    4158.57   57077.13
    Latency        5.70ms     4.30ms   369.69ms
    HTTP codes:
      1xx - 0, 2xx - 94002, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5998
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5998
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
    Reqs/sec      6429.72    1062.47    9356.89
    Latency        7.78ms     3.57ms   341.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     24158.23    6657.70   35678.77
    Latency        2.07ms     1.86ms   168.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     20874.54    5945.70   30526.87
    Latency        2.39ms     1.98ms   181.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     64143.38    4571.83   80615.71
    Latency      777.24us    70.81us     3.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.11MB/s
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
    Reqs/sec     18304.32    6314.25   54340.49
    Latency        2.72ms     2.34ms   203.72ms
    HTTP codes:
      1xx - 0, 2xx - 93827, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6173
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6173
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
    Reqs/sec     25717.95    7562.75   50889.10
    Latency        1.94ms     1.63ms   149.11ms
    HTTP codes:
      1xx - 0, 2xx - 97625, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2375
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2375
    Throughput:     5.76MB/s
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
    Reqs/sec     73912.14    5587.60   82351.48
    Latency      672.25us   220.74us    12.88ms
    HTTP codes:
      1xx - 0, 2xx - 96751, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3249
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3249
    Throughput:    11.32MB/s
  ```


