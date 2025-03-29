## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74646` | `5021` | `84440` |
| **83%** | [Hyper Express](#hyper-express) | `62261` | `3557` | `67153` |
| **37%** | [Node (Default)](#node-default) | `27989` | `8647` | `58818` |
| **32%** | [Fastify](#fastify) | `23993` | `7069` | `37914` |
| **28%** | [Hono](#hono) | `21236` | `5979` | `30797` |
| **25%** | [Koa](#koa) | `18435` | `6531` | `55067` |
| **11%** | [Carbon](#carbon) | `8377` | `1421` | `10408` |
| **9%** | [Express](#express) | `6529` | `1043` | `8571` |


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
    Reqs/sec      8949.48    4655.76   52426.46
    Latency        5.57ms     4.21ms   364.35ms
    HTTP codes:
      1xx - 0, 2xx - 92777, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7223
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7223
    Throughput:     1.89MB/s
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
    Reqs/sec      6996.16    4707.17   54371.77
    Latency        7.14ms     3.97ms   358.96ms
    HTTP codes:
      1xx - 0, 2xx - 91429, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8571
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8571
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
    Reqs/sec     24706.55    7279.33   36952.00
    Latency        2.02ms     1.91ms   173.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.61MB/s
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
    Reqs/sec     21361.93    6171.97   30718.62
    Latency        2.34ms     1.89ms   173.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     61202.97    2612.32   63882.37
    Latency      814.75us    62.38us     2.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.70MB/s
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
    Reqs/sec     19027.25    7169.75   57294.53
    Latency        2.62ms     2.44ms   212.52ms
    HTTP codes:
      1xx - 0, 2xx - 92880, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7120
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7120
    Throughput:     4.00MB/s
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
    Reqs/sec     27806.91    8382.28   47418.36
    Latency        1.79ms     1.83ms   159.73ms
    HTTP codes:
      1xx - 0, 2xx - 98015, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1985
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1985
    Throughput:     6.25MB/s
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
    Reqs/sec     73136.52    4578.76   81633.41
    Latency      680.92us   169.36us     8.89ms
    HTTP codes:
      1xx - 0, 2xx - 96799, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3201
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3201
    Throughput:    11.20MB/s
  ```


