## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70620` | `3825` | `82150` |
| **84%** | [Hyper Express](#hyper-express) | `59663` | `3605` | `65356` |
| **35%** | [Node (Default)](#node-default) | `24756` | `8235` | `75665` |
| **32%** | [Fastify](#fastify) | `22532` | `6968` | `35151` |
| **29%** | [Hono](#hono) | `20359` | `6315` | `30050` |
| **25%** | [Koa](#koa) | `17801` | `7564` | `62500` |
| **11%** | [Carbon](#carbon) | `7803` | `1314` | `10058` |
| **9%** | [Express](#express) | `6260` | `1079` | `8299` |


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
    Reqs/sec      8695.27    5474.17   72436.79
    Latency        5.74ms     4.33ms   374.26ms
    HTTP codes:
      1xx - 0, 2xx - 92323, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7677
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7677
    Throughput:     1.82MB/s
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
    Reqs/sec      6346.74    1079.33    8187.94
    Latency        7.87ms     3.63ms   352.90ms
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
    Reqs/sec     23116.94    7085.24   38062.57
    Latency        2.16ms     2.14ms   190.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.25MB/s
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
    Reqs/sec     20244.52    5800.41   29873.73
    Latency        2.47ms     2.18ms   194.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.57MB/s
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
    Reqs/sec     60069.96    3174.61   66778.08
    Latency      828.73us    79.39us     3.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.55MB/s
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
    Reqs/sec     18874.36    8204.26   65198.36
    Latency        2.64ms     2.49ms   216.57ms
    HTTP codes:
      1xx - 0, 2xx - 92082, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7918
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7918
    Throughput:     3.93MB/s
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
    Reqs/sec     24361.11    7173.59   59894.83
    Latency        2.05ms     2.02ms   173.13ms
    HTTP codes:
      1xx - 0, 2xx - 97455, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2545
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2545
    Throughput:     5.43MB/s
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
    Reqs/sec     70943.28    3192.80   80705.74
    Latency      702.28us   153.25us     6.86ms
    HTTP codes:
      1xx - 0, 2xx - 97214, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2786
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2786
    Throughput:    10.92MB/s
  ```


