## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73343` | `5092` | `77589` |
| **85%** | [Hyper Express](#hyper-express) | `62640` | `4307` | `77412` |
| **34%** | [Node (Default)](#node-default) | `25086` | `7362` | `50340` |
| **33%** | [Fastify](#fastify) | `24215` | `7402` | `36639` |
| **27%** | [Hono](#hono) | `19774` | `5323` | `29404` |
| **24%** | [Koa](#koa) | `17629` | `5856` | `44050` |
| **11%** | [Carbon](#carbon) | `8252` | `1442` | `10468` |
| **9%** | [Express](#express) | `6752` | `1122` | `8618` |


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
    Reqs/sec      8693.20    4298.56   55730.79
    Latency        5.74ms     4.17ms   362.79ms
    HTTP codes:
      1xx - 0, 2xx - 94075, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5925
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5925
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
    Reqs/sec      6536.57    1067.12    8638.92
    Latency        7.65ms     3.50ms   337.65ms
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
    Reqs/sec     23947.82    6789.13   35650.71
    Latency        2.09ms     1.99ms   180.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     20669.07    5432.99   29810.01
    Latency        2.42ms     2.00ms   178.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     62135.84    3755.10   73736.22
    Latency      802.76us    74.11us     4.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18111.81    6088.55   45608.46
    Latency        2.75ms     2.27ms   198.14ms
    HTTP codes:
      1xx - 0, 2xx - 95560, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4440
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4440
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
    Reqs/sec     26477.07    7799.68   52081.70
    Latency        1.88ms     1.91ms   161.73ms
    HTTP codes:
      1xx - 0, 2xx - 97099, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2901
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2901
    Throughput:     5.89MB/s
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
    Reqs/sec     73632.08    6152.43   90597.46
    Latency      676.85us   152.26us    17.61ms
    HTTP codes:
      1xx - 0, 2xx - 98263, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1737
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1737
    Throughput:    11.45MB/s
  ```


