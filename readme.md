## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73881` | `5614` | `83473` |
| **84%** | [Hyper Express](#hyper-express) | `62097` | `4157` | `69320` |
| **38%** | [Node (Default)](#node-default) | `28248` | `8886` | `53566` |
| **34%** | [Fastify](#fastify) | `25215` | `7695` | `36173` |
| **28%** | [Hono](#hono) | `20938` | `5816` | `30200` |
| **26%** | [Koa](#koa) | `18878` | `6929` | `53530` |
| **11%** | [Carbon](#carbon) | `8241` | `1446` | `10433` |
| **9%** | [Express](#express) | `6387` | `1059` | `8397` |


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
    Reqs/sec      8590.63    3989.48   49474.01
    Latency        5.81ms     4.36ms   378.09ms
    HTTP codes:
      1xx - 0, 2xx - 94503, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5497
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5497
    Throughput:     1.84MB/s
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
    Reqs/sec      6387.84    1038.85    8348.85
    Latency        7.82ms     3.73ms   357.16ms
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
    Reqs/sec     25908.84    8291.73   37736.37
    Latency        1.93ms     2.14ms   189.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.88MB/s
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
    Reqs/sec     20478.61    5486.92   29347.16
    Latency        2.44ms     2.03ms   180.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     63293.67    4807.32   75965.45
    Latency      787.74us    86.93us     3.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.99MB/s
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
    Reqs/sec     18644.65    7245.49   59371.91
    Latency        2.68ms     2.41ms   213.00ms
    HTTP codes:
      1xx - 0, 2xx - 92672, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7328
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7328
    Throughput:     3.91MB/s
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
    Reqs/sec     27175.07    8021.93   48601.19
    Latency        1.84ms     1.88ms   158.98ms
    HTTP codes:
      1xx - 0, 2xx - 97783, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2217
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2217
    Throughput:     6.08MB/s
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
    Reqs/sec     73965.62    5161.99   83763.41
    Latency      672.98us   157.21us     8.24ms
    HTTP codes:
      1xx - 0, 2xx - 97899, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2101
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2101
    Throughput:    11.46MB/s
  ```


