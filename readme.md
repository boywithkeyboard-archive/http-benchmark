## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74127` | `6471` | `93116` |
| **85%** | [Hyper Express](#hyper-express) | `63150` | `3783` | `73425` |
| **37%** | [Node (Default)](#node-default) | `27519` | `7861` | `53892` |
| **35%** | [Fastify](#fastify) | `25884` | `7869` | `37594` |
| **26%** | [Koa](#koa) | `19563` | `7408` | `55713` |
| **23%** | [Hono](#hono) | `17099` | `7946` | `29891` |
| **11%** | [Carbon](#carbon) | `8298` | `1411` | `10477` |
| **9%** | [Express](#express) | `6347` | `1007` | `9760` |


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
    Reqs/sec      8679.34    4881.43   60495.29
    Latency        5.75ms     4.24ms   366.09ms
    HTTP codes:
      1xx - 0, 2xx - 92894, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7106
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7106
    Throughput:     1.83MB/s
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
    Reqs/sec      7047.12    4081.02   52841.97
    Latency        7.09ms     4.02ms   366.26ms
    HTTP codes:
      1xx - 0, 2xx - 93110, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6890
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6890
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
    Reqs/sec     23756.53    7116.76   36332.85
    Latency        2.10ms     2.02ms   179.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.39MB/s
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
    Reqs/sec     21605.15    5956.16   30601.42
    Latency        2.31ms     2.05ms   182.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.88MB/s
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
    Reqs/sec     65071.81    4553.04   75108.56
    Latency      766.19us    72.44us     2.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.24MB/s
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
    Reqs/sec     18678.77    7056.42   58021.60
    Latency        2.67ms     2.36ms   203.15ms
    HTTP codes:
      1xx - 0, 2xx - 93502, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6498
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6498
    Throughput:     3.95MB/s
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
    Reqs/sec     27392.02    8883.23   62025.54
    Latency        1.82ms     1.97ms   167.29ms
    HTTP codes:
      1xx - 0, 2xx - 96713, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3287
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3287
    Throughput:     6.06MB/s
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
    Reqs/sec     72218.55    7419.60   85584.06
    Latency      688.78us   193.70us     7.00ms
    HTTP codes:
      1xx - 0, 2xx - 98135, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1865
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1865
    Throughput:    11.23MB/s
  ```


