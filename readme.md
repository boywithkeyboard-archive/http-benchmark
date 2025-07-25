## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75028` | `2701` | `81489` |
| **84%** | [Hyper Express](#hyper-express) | `62761` | `3481` | `66461` |
| **35%** | [Node (Default)](#node-default) | `26221` | `8507` | `67138` |
| **31%** | [Fastify](#fastify) | `23612` | `6944` | `35845` |
| **28%** | [Hono](#hono) | `20663` | `6173` | `30905` |
| **26%** | [Koa](#koa) | `19358` | `8251` | `67509` |
| **11%** | [Carbon](#carbon) | `8400` | `1477` | `10453` |
| **9%** | [Express](#express) | `6445` | `1086` | `8480` |


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
    Reqs/sec      8855.81    5583.83   69108.50
    Latency        5.63ms     4.28ms   368.46ms
    HTTP codes:
      1xx - 0, 2xx - 92524, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7476
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7476
    Throughput:     1.86MB/s
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
    Reqs/sec      6844.24    4888.95   64556.13
    Latency        7.29ms     3.95ms   361.63ms
    HTTP codes:
      1xx - 0, 2xx - 92134, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7866
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7866
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
    Reqs/sec     23273.41    6658.45   37087.92
    Latency        2.15ms     1.98ms   176.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.27MB/s
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
    Reqs/sec     20788.22    5292.42   30199.34
    Latency        2.40ms     2.10ms   188.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     62471.45    3450.15   65319.47
    Latency      798.12us    69.65us     2.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     18608.25    8968.58   75360.29
    Latency        2.68ms     2.43ms   213.99ms
    HTTP codes:
      1xx - 0, 2xx - 91233, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8767
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8767
    Throughput:     3.84MB/s
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
    Reqs/sec     24911.34    7093.26   64301.95
    Latency        2.00ms     1.90ms   164.31ms
    HTTP codes:
      1xx - 0, 2xx - 97286, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2714
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2714
    Throughput:     5.55MB/s
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
    Reqs/sec     75449.59    2808.76   83371.97
    Latency      660.45us   142.90us     6.47ms
    HTTP codes:
      1xx - 0, 2xx - 96923, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3077
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3077
    Throughput:    11.57MB/s
  ```


