## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74126` | `4739` | `80732` |
| **83%** | [Hyper Express](#hyper-express) | `61558` | `3586` | `67864` |
| **36%** | [Node (Default)](#node-default) | `26334` | `8133` | `55946` |
| **32%** | [Fastify](#fastify) | `23961` | `6938` | `36094` |
| **28%** | [Hono](#hono) | `20881` | `6067` | `30699` |
| **25%** | [Koa](#koa) | `18858` | `6879` | `57858` |
| **11%** | [Carbon](#carbon) | `8267` | `1401` | `10443` |
| **9%** | [Express](#express) | `6420` | `1047` | `8461` |


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
    Reqs/sec      8854.01    4777.23   57730.90
    Latency        5.64ms     4.29ms   372.54ms
    HTTP codes:
      1xx - 0, 2xx - 92856, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7144
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7144
    Throughput:     1.87MB/s
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
    Reqs/sec      7107.38    4447.66   50869.70
    Latency        7.02ms     3.95ms   358.36ms
    HTTP codes:
      1xx - 0, 2xx - 92023, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7977
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7977
    Throughput:     1.87MB/s
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
    Reqs/sec     24059.64    7287.69   36315.33
    Latency        2.08ms     2.09ms   185.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     20900.14    6052.13   29883.33
    Latency        2.39ms     1.93ms   176.07ms
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
    Reqs/sec     62165.80    3748.30   66019.35
    Latency      801.89us    72.77us     3.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     19127.28    7009.56   53728.72
    Latency        2.61ms     2.33ms   206.16ms
    HTTP codes:
      1xx - 0, 2xx - 93753, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6247
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6247
    Throughput:     4.06MB/s
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
    Reqs/sec     25741.21    7210.47   53893.69
    Latency        1.93ms     1.89ms   161.26ms
    HTTP codes:
      1xx - 0, 2xx - 97589, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2411
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2411
    Throughput:     5.76MB/s
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
    Reqs/sec     73619.04    4565.43   79296.06
    Latency      677.03us   138.72us     5.57ms
    HTTP codes:
      1xx - 0, 2xx - 97919, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2081
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2081
    Throughput:    11.41MB/s
  ```


