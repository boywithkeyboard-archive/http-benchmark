## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74223` | `5275` | `78673` |
| **84%** | [Hyper Express](#hyper-express) | `62532` | `3766` | `65299` |
| **35%** | [Node (Default)](#node-default) | `26138` | `7607` | `41435` |
| **33%** | [Fastify](#fastify) | `24227` | `6710` | `36464` |
| **29%** | [Hono](#hono) | `21836` | `6574` | `31043` |
| **24%** | [Koa](#koa) | `17797` | `5926` | `50933` |
| **11%** | [Carbon](#carbon) | `8529` | `1499` | `10640` |
| **9%** | [Express](#express) | `6563` | `1062` | `8567` |


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
    Reqs/sec      8828.47    4984.02   57699.22
    Latency        5.65ms     4.21ms   361.92ms
    HTTP codes:
      1xx - 0, 2xx - 92551, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7449
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7449
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
    Reqs/sec      6550.24    1028.29    8555.63
    Latency        7.63ms     3.53ms   338.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24843.10    7491.33   37953.49
    Latency        2.01ms     1.98ms   180.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.63MB/s
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
    Reqs/sec     20991.62    5950.32   30391.92
    Latency        2.38ms     1.99ms   178.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     62648.54    4053.20   68153.65
    Latency      795.83us    70.45us     4.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     19108.54    7000.93   56198.97
    Latency        2.61ms     2.34ms   203.70ms
    HTTP codes:
      1xx - 0, 2xx - 92816, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7184
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7184
    Throughput:     4.01MB/s
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
    Reqs/sec     27136.50    8277.88   42967.45
    Latency        1.84ms     1.83ms   160.59ms
    HTTP codes:
      1xx - 0, 2xx - 98109, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1891
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1891
    Throughput:     6.09MB/s
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
    Reqs/sec     73543.13    5528.54   83304.21
    Latency      675.91us   191.65us    15.19ms
    HTTP codes:
      1xx - 0, 2xx - 97357, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2643
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2643
    Throughput:    11.33MB/s
  ```


