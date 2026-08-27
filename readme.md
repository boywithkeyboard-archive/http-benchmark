## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79050` | `3287` | `84569` |
| **88%** | [Hyper Express](#hyper-express) | `69286` | `2751` | `72001` |
| **45%** | [Node (Default)](#node-default) | `35722` | `11218` | `90246` |
| **43%** | [Fastify](#fastify) | `33612` | `9379` | `52117` |
| **37%** | [Hono](#hono) | `29544` | `8609` | `47458` |
| **36%** | [Koa](#koa) | `28378` | `11489` | `87262` |
| **12%** | [Carbon](#carbon) | `9436` | `2384` | `13580` |
| **10%** | [Express](#express) | `7560` | `1696` | `10460` |


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
    Reqs/sec     10608.97    8793.95   84820.61
    Latency        4.69ms     4.24ms   365.50ms
    HTTP codes:
      1xx - 0, 2xx - 87635, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12365
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12365
    Throughput:     2.12MB/s
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
    Reqs/sec      8547.54    7558.76   77678.81
    Latency        5.84ms     3.83ms   350.85ms
    HTTP codes:
      1xx - 0, 2xx - 88582, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11418
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11418
    Throughput:     2.17MB/s
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
    Reqs/sec     33490.53    8601.70   48742.36
    Latency        1.49ms     2.05ms   177.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.60MB/s
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
    Reqs/sec     29513.49    8141.91   47730.75
    Latency        1.69ms     1.91ms   179.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.67MB/s
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
    Reqs/sec     69711.67    3270.21   73381.81
    Latency      715.57us    74.61us     2.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.90MB/s
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
    Reqs/sec     29563.87   12432.17   76460.52
    Latency        1.69ms     2.27ms   196.53ms
    HTTP codes:
      1xx - 0, 2xx - 92682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7318
    Throughput:     6.19MB/s
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
    Reqs/sec     36221.31   10481.75   70045.22
    Latency        1.38ms     1.63ms   139.21ms
    HTTP codes:
      1xx - 0, 2xx - 97153, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2847
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2847
    Throughput:     8.06MB/s
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
    Reqs/sec     78660.09    3275.19   83140.76
    Latency      632.68us   176.25us     8.46ms
    HTTP codes:
      1xx - 0, 2xx - 96509, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3491
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3491
    Throughput:    12.01MB/s
  ```


