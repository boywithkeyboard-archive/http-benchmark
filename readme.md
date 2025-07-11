## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73947` | `2455` | `79342` |
| **85%** | [Hyper Express](#hyper-express) | `63037` | `3640` | `67062` |
| **34%** | [Node (Default)](#node-default) | `25267` | `7634` | `65724` |
| **33%** | [Fastify](#fastify) | `24158` | `7272` | `35993` |
| **30%** | [Hono](#hono) | `22383` | `6710` | `31039` |
| **26%** | [Koa](#koa) | `18968` | `8856` | `73652` |
| **11%** | [Carbon](#carbon) | `8071` | `1416` | `10206` |
| **8%** | [Express](#express) | `6248` | `993` | `8328` |


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
    Reqs/sec      9000.37    6558.61   71599.48
    Latency        5.54ms     4.50ms   384.14ms
    HTTP codes:
      1xx - 0, 2xx - 90142, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9858
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9858
    Throughput:     1.84MB/s
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
    Reqs/sec      6866.01    4916.81   60076.41
    Latency        7.27ms     4.03ms   370.95ms
    HTTP codes:
      1xx - 0, 2xx - 91811, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8189
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8189
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
    Reqs/sec     24879.59    7980.50   36854.60
    Latency        2.01ms     2.10ms   186.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.65MB/s
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
    Reqs/sec     21104.35    6048.50   29795.70
    Latency        2.37ms     2.01ms   178.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     63455.32    3189.51   66707.33
    Latency      785.55us    67.21us     2.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.02MB/s
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
    Reqs/sec     18612.02    8672.19   74281.04
    Latency        2.68ms     2.47ms   215.33ms
    HTTP codes:
      1xx - 0, 2xx - 91509, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8491
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8491
    Throughput:     3.85MB/s
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
    Reqs/sec     26091.08    8487.20   81921.27
    Latency        1.91ms     1.94ms   165.08ms
    HTTP codes:
      1xx - 0, 2xx - 96463, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3537
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3537
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
    Reqs/sec     73124.22    2982.00   79511.87
    Latency      681.53us   146.12us     6.88ms
    HTTP codes:
      1xx - 0, 2xx - 97155, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2845
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2845
    Throughput:    11.24MB/s
  ```


