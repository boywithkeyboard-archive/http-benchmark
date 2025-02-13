## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74476` | `6435` | `92988` |
| **86%** | [Hyper Express](#hyper-express) | `63719` | `3931` | `71102` |
| **34%** | [Node (Default)](#node-default) | `25435` | `7449` | `52867` |
| **32%** | [Fastify](#fastify) | `23817` | `6972` | `36402` |
| **28%** | [Hono](#hono) | `20856` | `6000` | `29547` |
| **25%** | [Koa](#koa) | `18663` | `6631` | `52523` |
| **11%** | [Carbon](#carbon) | `8247` | `1397` | `10618` |
| **9%** | [Express](#express) | `6352` | `1007` | `8806` |


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
    Reqs/sec      8775.31    4928.21   57506.41
    Latency        5.69ms     4.35ms   378.39ms
    HTTP codes:
      1xx - 0, 2xx - 92703, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7297
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7297
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
    Reqs/sec      6837.10    3954.04   47546.49
    Latency        7.30ms     3.93ms   357.89ms
    HTTP codes:
      1xx - 0, 2xx - 93063, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6937
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6937
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
    Reqs/sec     24369.98    7431.80   35972.89
    Latency        2.05ms     2.02ms   181.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.53MB/s
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
    Reqs/sec     20878.26    5659.29   29592.10
    Latency        2.39ms     2.12ms   186.69ms
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
    Reqs/sec     64683.12    4345.27   74111.24
    Latency      771.27us    73.85us     3.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.18MB/s
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
    Reqs/sec     18406.65    6376.72   51839.76
    Latency        2.71ms     2.36ms   208.26ms
    HTTP codes:
      1xx - 0, 2xx - 94315, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5685
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5685
    Throughput:     3.92MB/s
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
    Reqs/sec     26517.41    7611.08   52579.27
    Latency        1.88ms     1.86ms   160.24ms
    HTTP codes:
      1xx - 0, 2xx - 97820, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2180
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2180
    Throughput:     5.94MB/s
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
    Reqs/sec     74172.22    5323.93   88327.24
    Latency      671.64us   195.65us     7.11ms
    HTTP codes:
      1xx - 0, 2xx - 95602, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4398
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4398
    Throughput:    11.22MB/s
  ```


