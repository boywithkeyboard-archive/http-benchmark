## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75433` | `2458` | `79676` |
| **83%** | [Hyper Express](#hyper-express) | `62900` | `3543` | `66652` |
| **35%** | [Node (Default)](#node-default) | `26594` | `8616` | `65164` |
| **32%** | [Fastify](#fastify) | `24170` | `7325` | `37524` |
| **28%** | [Hono](#hono) | `21375` | `5764` | `31164` |
| **25%** | [Koa](#koa) | `18516` | `8626` | `78873` |
| **11%** | [Carbon](#carbon) | `8292` | `1454` | `10505` |
| **9%** | [Express](#express) | `6441` | `1064` | `8392` |


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
    Reqs/sec      8860.16    5815.98   65564.31
    Latency        5.64ms     4.22ms   366.03ms
    HTTP codes:
      1xx - 0, 2xx - 91684, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8316
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8316
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
    Reqs/sec      7060.66    5705.06   67853.27
    Latency        7.07ms     3.99ms   365.62ms
    HTTP codes:
      1xx - 0, 2xx - 90452, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9548
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9548
    Throughput:     1.83MB/s
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
    Reqs/sec     23860.91    7269.96   36631.06
    Latency        2.09ms     2.05ms   182.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     20884.76    5616.73   29937.20
    Latency        2.39ms     2.03ms   182.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     63152.23    3480.02   65810.79
    Latency      789.84us    66.73us     3.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     19181.74    8945.79   77738.44
    Latency        2.60ms     2.38ms   210.09ms
    HTTP codes:
      1xx - 0, 2xx - 91506, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8494
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8494
    Throughput:     3.97MB/s
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
    Reqs/sec     27066.56    8948.84   74559.72
    Latency        1.84ms     2.01ms   166.87ms
    HTTP codes:
      1xx - 0, 2xx - 96472, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3528
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3528
    Throughput:     5.98MB/s
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
    Reqs/sec     74819.43    3301.48   82550.59
    Latency      665.93us   137.19us     6.25ms
    HTTP codes:
      1xx - 0, 2xx - 97189, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2811
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2811
    Throughput:    11.51MB/s
  ```


