## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71390` | `3158` | `80420` |
| **84%** | [Hyper Express](#hyper-express) | `59956` | `3417` | `65966` |
| **35%** | [Node (Default)](#node-default) | `24885` | `8167` | `76091` |
| **32%** | [Fastify](#fastify) | `22894` | `6570` | `35177` |
| **29%** | [Hono](#hono) | `21022` | `6109` | `30101` |
| **27%** | [Koa](#koa) | `18922` | `7595` | `61233` |
| **11%** | [Carbon](#carbon) | `8167` | `1470` | `10393` |
| **9%** | [Express](#express) | `6398` | `1116` | `8383` |


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
    Reqs/sec      8919.56    6602.74   75124.19
    Latency        5.59ms     4.33ms   375.87ms
    HTTP codes:
      1xx - 0, 2xx - 90104, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9896
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9896
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
    Reqs/sec      6338.64    1073.38    8316.62
    Latency        7.88ms     3.77ms   361.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23074.66    6538.87   36296.52
    Latency        2.17ms     2.11ms   189.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.23MB/s
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
    Reqs/sec     21258.06    6271.00   30860.78
    Latency        2.35ms     2.08ms   189.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     60010.65    3334.14   66640.62
    Latency      831.37us    80.41us     3.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.52MB/s
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
    Reqs/sec     19600.98    8820.61   70545.49
    Latency        2.55ms     2.41ms   212.83ms
    HTTP codes:
      1xx - 0, 2xx - 92064, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7936
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7936
    Throughput:     4.08MB/s
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
    Reqs/sec     23943.63    6140.04   64591.41
    Latency        2.08ms     2.07ms   176.56ms
    HTTP codes:
      1xx - 0, 2xx - 96977, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3023
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3023
    Throughput:     5.32MB/s
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
    Reqs/sec     71263.98    3466.52   79400.67
    Latency      699.94us   198.07us    11.47ms
    HTTP codes:
      1xx - 0, 2xx - 96993, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3007
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3007
    Throughput:    10.93MB/s
  ```


