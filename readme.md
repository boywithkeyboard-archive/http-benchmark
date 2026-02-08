## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75177` | `2229` | `80533` |
| **79%** | [Hyper Express](#hyper-express) | `59286` | `7981` | `65258` |
| **33%** | [Node (Default)](#node-default) | `24529` | `7387` | `63906` |
| **31%** | [Fastify](#fastify) | `23435` | `6795` | `35389` |
| **27%** | [Hono](#hono) | `20072` | `6207` | `29653` |
| **24%** | [Koa](#koa) | `18214` | `8914` | `78947` |
| **11%** | [Carbon](#carbon) | `7916` | `1354` | `10122` |
| **8%** | [Express](#express) | `6291` | `1038` | `8288` |


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
    Reqs/sec      8909.90    6088.39   73626.68
    Latency        5.61ms     4.48ms   387.66ms
    HTTP codes:
      1xx - 0, 2xx - 91484, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8516
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8516
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
    Reqs/sec      6132.87    1002.76    8298.61
    Latency        8.15ms     3.83ms   369.71ms
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
    Reqs/sec     23979.95    7844.07   35142.75
    Latency        2.08ms     2.04ms   184.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.44MB/s
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
    Reqs/sec     20260.10    5869.92   29372.07
    Latency        2.47ms     2.11ms   189.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     61976.23    4053.29   68749.87
    Latency      804.77us    79.14us     3.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     18911.46    8760.94   75282.17
    Latency        2.64ms     2.42ms   213.44ms
    HTTP codes:
      1xx - 0, 2xx - 92013, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7987
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7987
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
    Reqs/sec     26115.05    8500.22   65064.16
    Latency        1.91ms     1.87ms   162.35ms
    HTTP codes:
      1xx - 0, 2xx - 97282, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2718
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2718
    Throughput:     5.81MB/s
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
    Reqs/sec     74840.94    2171.59   78747.41
    Latency      665.84us   138.29us     5.37ms
    HTTP codes:
      1xx - 0, 2xx - 97266, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2734
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2734
    Throughput:    11.52MB/s
  ```


