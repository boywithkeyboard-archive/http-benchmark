## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70719` | `4036` | `81637` |
| **82%** | [Hyper Express](#hyper-express) | `58136` | `3207` | `63776` |
| **31%** | [Fastify](#fastify) | `21684` | `6076` | `36644` |
| **30%** | [Hono](#hono) | `21418` | `6696` | `31039` |
| **30%** | [Node (Default)](#node-default) | `21126` | `5152` | `58885` |
| **25%** | [Koa](#koa) | `17914` | `8620` | `69301` |
| **11%** | [Carbon](#carbon) | `7568` | `1298` | `10524` |
| **9%** | [Express](#express) | `6394` | `1137` | `8386` |


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
    Reqs/sec      8312.73    6382.34   69262.12
    Latency        6.00ms     4.68ms   392.73ms
    HTTP codes:
      1xx - 0, 2xx - 89272, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10728
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10728
    Throughput:     1.69MB/s
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
    Reqs/sec      6137.52    1033.19    8322.29
    Latency        8.14ms     3.94ms   378.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     21078.87    5617.24   36462.99
    Latency        2.37ms     2.10ms   187.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     21660.54    6557.02   31078.63
    Latency        2.31ms     2.32ms   204.36ms
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
    Reqs/sec     57590.94    3560.08   64715.53
    Latency        0.87ms    95.02us     3.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.18MB/s
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
    Reqs/sec     17469.54    7359.76   58108.72
    Latency        2.85ms     2.59ms   224.14ms
    HTTP codes:
      1xx - 0, 2xx - 93818, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6182
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6182
    Throughput:     3.71MB/s
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
    Reqs/sec     20247.17    5085.60   62267.15
    Latency        2.46ms     1.95ms   169.49ms
    HTTP codes:
      1xx - 0, 2xx - 96848, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3152
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3152
    Throughput:     4.49MB/s
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
    Reqs/sec     69755.80    3906.07   79806.30
    Latency      714.53us   152.70us     7.37ms
    HTTP codes:
      1xx - 0, 2xx - 97723, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2277
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2277
    Throughput:    10.78MB/s
  ```


