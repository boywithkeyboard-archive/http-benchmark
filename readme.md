## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69664` | `3828` | `83653` |
| **81%** | [Hyper Express](#hyper-express) | `56270` | `3880` | `66946` |
| **29%** | [Hono](#hono) | `20454` | `6280` | `30628` |
| **29%** | [Node (Default)](#node-default) | `20185` | `5880` | `67447` |
| **28%** | [Fastify](#fastify) | `19577` | `4494` | `34124` |
| **26%** | [Koa](#koa) | `18203` | `8126` | `65015` |
| **11%** | [Carbon](#carbon) | `7334` | `1249` | `10177` |
| **9%** | [Express](#express) | `6096` | `1085` | `8107` |


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
    Reqs/sec      7705.66    4533.54   58449.83
    Latency        6.47ms     4.66ms   392.83ms
    HTTP codes:
      1xx - 0, 2xx - 93464, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6536
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6536
    Throughput:     1.64MB/s
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
    Reqs/sec      6053.27    1065.28    8159.17
    Latency        8.26ms     3.93ms   373.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     19750.97    4574.76   36900.05
    Latency        2.53ms     2.25ms   203.81ms
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
    Reqs/sec     21004.72    6510.80   29288.27
    Latency        2.38ms     2.36ms   205.23ms
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
    Reqs/sec     58027.83    4085.37   65661.98
    Latency        0.86ms   102.21us     5.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.24MB/s
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
    Reqs/sec     17540.13    8743.41   79045.36
    Latency        2.84ms     2.40ms   210.37ms
    HTTP codes:
      1xx - 0, 2xx - 91328, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8672
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8672
    Throughput:     3.63MB/s
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
    Reqs/sec     19715.78    5075.80   64960.75
    Latency        2.53ms     1.98ms   170.23ms
    HTTP codes:
      1xx - 0, 2xx - 96898, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3102
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3102
    Throughput:     4.38MB/s
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
    Reqs/sec     69724.04    3790.90   81833.41
    Latency      713.93us   161.59us     9.14ms
    HTTP codes:
      1xx - 0, 2xx - 96815, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3185
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3185
    Throughput:    10.68MB/s
  ```


