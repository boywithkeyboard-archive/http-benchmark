## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74535` | `6118` | `85763` |
| **82%** | [Hyper Express](#hyper-express) | `60975` | `3648` | `63245` |
| **37%** | [Node (Default)](#node-default) | `27794` | `8854` | `62427` |
| **33%** | [Fastify](#fastify) | `24665` | `7530` | `36373` |
| **29%** | [Hono](#hono) | `21932` | `5908` | `30549` |
| **25%** | [Koa](#koa) | `18881` | `6419` | `50683` |
| **11%** | [Carbon](#carbon) | `8384` | `1431` | `10500` |
| **9%** | [Express](#express) | `6508` | `1047` | `8439` |


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
    Reqs/sec      8827.81    4244.58   52416.96
    Latency        5.65ms     4.20ms   363.87ms
    HTTP codes:
      1xx - 0, 2xx - 94212, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5788
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5788
    Throughput:     1.89MB/s
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
    Reqs/sec      7027.54    4378.80   57803.39
    Latency        7.11ms     3.91ms   354.44ms
    HTTP codes:
      1xx - 0, 2xx - 92603, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7397
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7397
    Throughput:     1.86MB/s
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
    Reqs/sec     24742.82    7653.53   37101.21
    Latency        2.02ms     2.03ms   183.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.61MB/s
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
    Reqs/sec     21026.02    5524.00   30397.00
    Latency        2.38ms     1.99ms   178.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     61483.46    3781.15   68989.38
    Latency      811.31us    73.57us     2.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.73MB/s
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
    Reqs/sec     20284.82    8335.41   65269.42
    Latency        2.46ms     2.36ms   205.77ms
    HTTP codes:
      1xx - 0, 2xx - 91217, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8783
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8783
    Throughput:     4.18MB/s
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
    Reqs/sec     27929.29    8352.41   50019.89
    Latency        1.78ms     1.86ms   157.64ms
    HTTP codes:
      1xx - 0, 2xx - 97795, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2205
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2205
    Throughput:     6.26MB/s
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
    Reqs/sec     73739.01    5669.55   86495.95
    Latency      675.63us   153.09us    11.80ms
    HTTP codes:
      1xx - 0, 2xx - 97955, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2045
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2045
    Throughput:    11.43MB/s
  ```


