## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72792` | `5859` | `101882` |
| **81%** | [Hyper Express](#hyper-express) | `58813` | `2656` | `64282` |
| **29%** | [Hono](#hono) | `21402` | `6496` | `30582` |
| **28%** | [Fastify](#fastify) | `20297` | `4761` | `35469` |
| **28%** | [Node (Default)](#node-default) | `20157` | `5138` | `57386` |
| **24%** | [Koa](#koa) | `17563` | `7414` | `62219` |
| **11%** | [Carbon](#carbon) | `7682` | `1374` | `10485` |
| **9%** | [Express](#express) | `6206` | `1116` | `8641` |


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
    Reqs/sec      8154.10    5247.69   62801.16
    Latency        6.10ms     4.57ms   387.28ms
    HTTP codes:
      1xx - 0, 2xx - 91613, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8387
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8387
    Throughput:     1.70MB/s
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
    Reqs/sec      6349.87    1141.23    8339.41
    Latency        7.87ms     3.83ms   366.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     20444.55    4816.76   35620.50
    Latency        2.44ms     2.09ms   188.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.64MB/s
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
    Reqs/sec     22616.62    6600.18   31169.18
    Latency        2.21ms     2.22ms   197.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.11MB/s
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
    Reqs/sec     59142.35    3563.85   66210.77
    Latency      842.81us    93.14us     3.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.40MB/s
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
    Reqs/sec     17743.02    7359.12   57872.67
    Latency        2.81ms     2.45ms   213.10ms
    HTTP codes:
      1xx - 0, 2xx - 93661, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6339
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6339
    Throughput:     3.76MB/s
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
    Reqs/sec     20421.70    4894.02   57640.82
    Latency        2.44ms     2.04ms   172.52ms
    HTTP codes:
      1xx - 0, 2xx - 96982, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3018
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3018
    Throughput:     4.53MB/s
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
    Reqs/sec     70425.05    3609.61   81935.53
    Latency      707.44us   146.83us     5.33ms
    HTTP codes:
      1xx - 0, 2xx - 97078, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2922
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2922
    Throughput:    10.82MB/s
  ```


