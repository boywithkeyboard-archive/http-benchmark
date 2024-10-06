## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74301` | `5435` | `85504` |
| **84%** | [Hyper Express](#hyper-express) | `62703` | `3975` | `84099` |
| **36%** | [Node (Default)](#node-default) | `26676` | `7537` | `45295` |
| **33%** | [Fastify](#fastify) | `24218` | `7696` | `36248` |
| **28%** | [Hono](#hono) | `20435` | `5576` | `29413` |
| **25%** | [Koa](#koa) | `18626` | `6770` | `53436` |
| **11%** | [Carbon](#carbon) | `8297` | `1449` | `10385` |
| **9%** | [Express](#express) | `6444` | `1060` | `8340` |


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
    Reqs/sec      8755.77    5130.21   62565.21
    Latency        5.70ms     4.31ms   373.56ms
    HTTP codes:
      1xx - 0, 2xx - 92523, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7477
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7477
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
    Reqs/sec      6244.88     996.55    8054.54
    Latency        8.00ms     3.73ms   357.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     24255.35    7534.67   36162.58
    Latency        2.06ms     2.04ms   185.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     20887.41    5227.88   29274.29
    Latency        2.39ms     2.01ms   182.74ms
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
    Reqs/sec     63369.48    4181.54   75237.41
    Latency      786.77us    69.24us     3.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     18598.54    6591.52   56067.95
    Latency        2.68ms     2.37ms   208.91ms
    HTTP codes:
      1xx - 0, 2xx - 94268, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5732
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5732
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
    Reqs/sec     26751.17    7727.95   51996.50
    Latency        1.87ms     1.86ms   161.53ms
    HTTP codes:
      1xx - 0, 2xx - 97714, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2286
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2286
    Throughput:     5.98MB/s
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
    Reqs/sec     73990.64    7163.27   85181.53
    Latency      673.74us   214.79us     9.80ms
    HTTP codes:
      1xx - 0, 2xx - 98129, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1871
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1871
    Throughput:    11.48MB/s
  ```


