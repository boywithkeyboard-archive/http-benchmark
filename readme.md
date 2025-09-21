## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75184` | `2713` | `80290` |
| **83%** | [Hyper Express](#hyper-express) | `62035` | `4214` | `66653` |
| **35%** | [Node (Default)](#node-default) | `25993` | `8122` | `75459` |
| **31%** | [Fastify](#fastify) | `23664` | `7339` | `37340` |
| **28%** | [Hono](#hono) | `20854` | `5851` | `30738` |
| **25%** | [Koa](#koa) | `18859` | `8093` | `66991` |
| **11%** | [Carbon](#carbon) | `8294` | `1441` | `10547` |
| **9%** | [Express](#express) | `6419` | `1072` | `8503` |


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
    Reqs/sec      8666.95    6316.74   74506.78
    Latency        5.76ms     4.22ms   365.77ms
    HTTP codes:
      1xx - 0, 2xx - 90465, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9535
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9535
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
    Reqs/sec      6431.73    1036.31    8419.59
    Latency        7.77ms     3.65ms   350.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     23401.35    6532.61   36154.59
    Latency        2.13ms     2.02ms   185.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.31MB/s
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
    Reqs/sec     21020.20    6166.34   30980.84
    Latency        2.38ms     2.10ms   186.72ms
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
    Reqs/sec     63360.56    3022.14   67756.92
    Latency      786.93us    67.75us     2.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     18810.17    8100.31   66725.29
    Latency        2.65ms     2.40ms   209.96ms
    HTTP codes:
      1xx - 0, 2xx - 93217, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6783
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6783
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
    Reqs/sec     25608.76    8072.12   74538.59
    Latency        1.95ms     1.94ms   163.25ms
    HTTP codes:
      1xx - 0, 2xx - 96465, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3535
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3535
    Throughput:     5.65MB/s
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
    Reqs/sec     75143.53    2843.18   83624.23
    Latency      662.01us   159.18us     8.04ms
    HTTP codes:
      1xx - 0, 2xx - 96602, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3398
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3398
    Throughput:    11.49MB/s
  ```


