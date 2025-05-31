## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72898` | `5779` | `79570` |
| **85%** | [Hyper Express](#hyper-express) | `62008` | `3286` | `64463` |
| **35%** | [Node (Default)](#node-default) | `25773` | `7941` | `57324` |
| **32%** | [Fastify](#fastify) | `23653` | `7079` | `36053` |
| **29%** | [Hono](#hono) | `20831` | `5892` | `30437` |
| **25%** | [Koa](#koa) | `18201` | `6493` | `58330` |
| **11%** | [Carbon](#carbon) | `8343` | `1451` | `10393` |
| **9%** | [Express](#express) | `6348` | `1030` | `8419` |


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
    Reqs/sec      8507.37    4137.96   53882.18
    Latency        5.86ms     4.37ms   379.65ms
    HTTP codes:
      1xx - 0, 2xx - 94172, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5828
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5828
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
    Reqs/sec      6160.60     994.91    8236.57
    Latency        8.11ms     3.85ms   369.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     23180.45    6745.91   35382.67
    Latency        2.15ms     2.06ms   182.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.26MB/s
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
    Reqs/sec     20853.72    5879.16   29360.22
    Latency        2.40ms     2.05ms   185.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     62285.37    3694.00   66787.87
    Latency      801.22us    71.78us     3.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18046.78    6766.23   58873.57
    Latency        2.77ms     2.56ms   225.85ms
    HTTP codes:
      1xx - 0, 2xx - 92546, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7454
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7454
    Throughput:     3.77MB/s
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
    Reqs/sec     25958.08    8098.49   51152.18
    Latency        1.92ms     2.04ms   177.72ms
    HTTP codes:
      1xx - 0, 2xx - 97733, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2267
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2267
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
    Reqs/sec     72278.88    5284.81   80784.96
    Latency      688.77us   181.52us     8.46ms
    HTTP codes:
      1xx - 0, 2xx - 97500, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2500
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2500
    Throughput:    11.16MB/s
  ```


