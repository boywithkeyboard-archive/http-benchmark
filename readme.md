## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69716` | `3452` | `78931` |
| **81%** | [Hyper Express](#hyper-express) | `56818` | `2922` | `66001` |
| **31%** | [Node (Default)](#node-default) | `21718` | `5320` | `59965` |
| **30%** | [Fastify](#fastify) | `20757` | `4890` | `36513` |
| **28%** | [Hono](#hono) | `19770` | `5958` | `30632` |
| **26%** | [Koa](#koa) | `17804` | `7813` | `62390` |
| **11%** | [Carbon](#carbon) | `7840` | `1465` | `10350` |
| **9%** | [Express](#express) | `6107` | `1084` | `8386` |


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
    Reqs/sec      8669.72    6425.59   69143.37
    Latency        5.75ms     4.44ms   380.35ms
    HTTP codes:
      1xx - 0, 2xx - 90157, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9843
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9843
    Throughput:     1.78MB/s
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
    Reqs/sec      6336.55    1184.30    8466.05
    Latency        7.89ms     3.72ms   357.46ms
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
    Reqs/sec     20991.51    5098.77   37872.45
    Latency        2.38ms     2.19ms   196.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     21669.03    6731.32   30571.54
    Latency        2.31ms     2.12ms   191.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.89MB/s
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
    Reqs/sec     56900.67    3456.78   70949.45
    Latency        0.88ms    89.11us     5.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.06MB/s
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
    Reqs/sec     18583.40    8830.38   71729.31
    Latency        2.68ms     2.49ms   221.37ms
    HTTP codes:
      1xx - 0, 2xx - 92508, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7492
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7492
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
    Reqs/sec     21366.95    5686.60   73417.22
    Latency        2.34ms     2.05ms   173.75ms
    HTTP codes:
      1xx - 0, 2xx - 96729, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3271
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3271
    Throughput:     4.73MB/s
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
    Reqs/sec     69864.64    3640.51   78849.75
    Latency      713.60us   153.32us     5.75ms
    HTTP codes:
      1xx - 0, 2xx - 96947, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3053
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3053
    Throughput:    10.71MB/s
  ```


