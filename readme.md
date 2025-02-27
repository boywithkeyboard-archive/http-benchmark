## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72862` | `6849` | `83113` |
| **84%** | [Hyper Express](#hyper-express) | `61351` | `4365` | `65512` |
| **34%** | [Node (Default)](#node-default) | `25046` | `7062` | `49479` |
| **34%** | [Fastify](#fastify) | `24818` | `7676` | `37276` |
| **28%** | [Hono](#hono) | `20494` | `6029` | `29373` |
| **25%** | [Koa](#koa) | `18571` | `6546` | `51871` |
| **11%** | [Carbon](#carbon) | `8097` | `1446` | `10262` |
| **8%** | [Express](#express) | `6169` | `1039` | `8361` |


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
    Reqs/sec      8369.99    4860.97   53864.08
    Latency        5.95ms     4.64ms   401.28ms
    HTTP codes:
      1xx - 0, 2xx - 92043, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7957
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7957
    Throughput:     1.75MB/s
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
    Reqs/sec      6135.92    1020.79    8343.02
    Latency        8.15ms     4.26ms   396.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     23964.96    7402.97   36006.80
    Latency        2.09ms     2.13ms   193.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     21197.26    6091.94   30354.77
    Latency        2.36ms     2.11ms   186.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     62080.77    4718.37   68378.25
    Latency      803.06us    86.03us     3.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18759.53    6737.92   52399.85
    Latency        2.66ms     2.39ms   207.26ms
    HTTP codes:
      1xx - 0, 2xx - 93465, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6535
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6535
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
    Reqs/sec     25117.97    7883.05   59457.67
    Latency        1.99ms     2.06ms   171.15ms
    HTTP codes:
      1xx - 0, 2xx - 96634, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3366
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3366
    Throughput:     5.56MB/s
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
    Reqs/sec     71836.70    4859.62   78017.85
    Latency      693.68us   158.02us     5.34ms
    HTTP codes:
      1xx - 0, 2xx - 97529, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2471
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2471
    Throughput:    11.09MB/s
  ```


