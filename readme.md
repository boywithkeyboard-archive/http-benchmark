## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75633` | `5497` | `81699` |
| **84%** | [Hyper Express](#hyper-express) | `63818` | `4008` | `68439` |
| **31%** | [Node (Default)](#node-default) | `23721` | `6880` | `65563` |
| **29%** | [Fastify](#fastify) | `21657` | `6023` | `36403` |
| **25%** | [Hono](#hono) | `18604` | `4503` | `28931` |
| **21%** | [Koa](#koa) | `16097` | `5529` | `53558` |
| **10%** | [Carbon](#carbon) | `7774` | `1259` | `10281` |
| **8%** | [Express](#express) | `6344` | `997` | `8364` |


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
    Reqs/sec      8198.23    3345.89   46223.41
    Latency        6.09ms     4.24ms   370.69ms
    HTTP codes:
      1xx - 0, 2xx - 95605, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4395
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4395
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
    Reqs/sec      6390.90    1020.30    8339.23
    Latency        7.82ms     3.63ms   349.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     21045.06    5438.13   35976.07
    Latency        2.37ms     2.15ms   193.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     18019.30    4319.22   29127.90
    Latency        2.77ms     2.05ms   185.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.07MB/s
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
    Reqs/sec     63893.13    4121.77   74906.33
    Latency      780.63us    70.15us     4.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.07MB/s
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
    Reqs/sec     16417.29    5350.05   53600.26
    Latency        3.04ms     2.38ms   210.40ms
    HTTP codes:
      1xx - 0, 2xx - 94014, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5986
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5986
    Throughput:     3.49MB/s
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
    Reqs/sec     22669.95    5451.24   42616.40
    Latency        2.20ms     1.90ms   164.68ms
    HTTP codes:
      1xx - 0, 2xx - 98310, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1690
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1690
    Throughput:     5.11MB/s
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
    Reqs/sec     76322.96    6240.87   83497.39
    Latency      652.67us   217.47us    14.47ms
    HTTP codes:
      1xx - 0, 2xx - 97925, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2075
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2075
    Throughput:    11.83MB/s
  ```


