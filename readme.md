## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75509` | `3178` | `87221` |
| **83%** | [Hyper Express](#hyper-express) | `62357` | `3144` | `67769` |
| **34%** | [Node (Default)](#node-default) | `25459` | `7988` | `79536` |
| **31%** | [Fastify](#fastify) | `23578` | `7142` | `35693` |
| **27%** | [Hono](#hono) | `20719` | `5797` | `29962` |
| **24%** | [Koa](#koa) | `18045` | `8408` | `77421` |
| **11%** | [Carbon](#carbon) | `8245` | `1460` | `10386` |
| **8%** | [Express](#express) | `6403` | `1115` | `8409` |


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
    Reqs/sec      9187.12    7288.00   82486.55
    Latency        5.43ms     4.27ms   367.43ms
    HTTP codes:
      1xx - 0, 2xx - 89220, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10780
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10780
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
    Reqs/sec      7079.19    6139.58   79579.12
    Latency        7.05ms     3.96ms   361.49ms
    HTTP codes:
      1xx - 0, 2xx - 89630, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10370
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10370
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
    Reqs/sec     23237.37    6929.62   34820.65
    Latency        2.15ms     2.12ms   187.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.27MB/s
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
    Reqs/sec     20985.19    5982.45   29904.09
    Latency        2.38ms     2.02ms   182.45ms
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
    Reqs/sec     62559.08    3335.20   65714.43
    Latency      797.04us    73.21us     3.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     18777.67    9635.94   80567.75
    Latency        2.66ms     2.31ms   204.97ms
    HTTP codes:
      1xx - 0, 2xx - 90287, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9713
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9713
    Throughput:     3.84MB/s
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
    Reqs/sec     25777.58    7855.95   60308.33
    Latency        1.94ms     1.84ms   162.38ms
    HTTP codes:
      1xx - 0, 2xx - 97522, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2478
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2478
    Throughput:     5.75MB/s
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
    Reqs/sec     74599.29    1784.15   79265.85
    Latency      667.26us   159.17us     9.97ms
    HTTP codes:
      1xx - 0, 2xx - 96035, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3965
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3965
    Throughput:    11.34MB/s
  ```


