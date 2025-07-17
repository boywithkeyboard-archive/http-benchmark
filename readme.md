## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73647` | `2147` | `80962` |
| **83%** | [Hyper Express](#hyper-express) | `61310` | `4135` | `66006` |
| **35%** | [Node (Default)](#node-default) | `25507` | `7731` | `59534` |
| **33%** | [Fastify](#fastify) | `24578` | `7860` | `36075` |
| **28%** | [Hono](#hono) | `20363` | `5529` | `30045` |
| **25%** | [Koa](#koa) | `18777` | `8086` | `66439` |
| **11%** | [Carbon](#carbon) | `8068` | `1401` | `10425` |
| **9%** | [Express](#express) | `6373` | `1073` | `8423` |


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
    Reqs/sec      8763.91    5391.05   68245.05
    Latency        5.69ms     4.40ms   380.35ms
    HTTP codes:
      1xx - 0, 2xx - 92656, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7344
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7344
    Throughput:     1.85MB/s
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
    Reqs/sec      6220.19     982.89    8412.82
    Latency        8.04ms     3.69ms   356.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     23904.74    7271.45   36138.14
    Latency        2.09ms     1.94ms   176.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     20228.18    5883.96   29871.09
    Latency        2.47ms     2.05ms   185.78ms
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
    Reqs/sec     62267.15    3374.56   65372.79
    Latency      800.14us    72.60us     2.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.85MB/s
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
    Reqs/sec     19301.59   10841.68   83001.13
    Latency        2.58ms     2.39ms   211.83ms
    HTTP codes:
      1xx - 0, 2xx - 87179, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12821
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12821
    Throughput:     3.81MB/s
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
    Reqs/sec     26194.72    8721.96   67862.96
    Latency        1.90ms     2.10ms   180.10ms
    HTTP codes:
      1xx - 0, 2xx - 96701, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3299
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3299
    Throughput:     5.80MB/s
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
    Reqs/sec     74139.08    2254.84   81175.07
    Latency      671.38us   179.79us    10.47ms
    HTTP codes:
      1xx - 0, 2xx - 96303, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3697
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3697
    Throughput:    11.30MB/s
  ```


