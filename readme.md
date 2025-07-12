## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75924` | `1987` | `79915` |
| **85%** | [Hyper Express](#hyper-express) | `64689` | `3342` | `67931` |
| **33%** | [Node (Default)](#node-default) | `25162` | `8070` | `69210` |
| **32%** | [Fastify](#fastify) | `24618` | `7582` | `36295` |
| **28%** | [Hono](#hono) | `21324` | `5970` | `29764` |
| **25%** | [Koa](#koa) | `19310` | `8230` | `67672` |
| **11%** | [Carbon](#carbon) | `8385` | `1446` | `10458` |
| **9%** | [Express](#express) | `6620` | `1107` | `8534` |


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
    Reqs/sec      9034.23    6362.38   77179.43
    Latency        5.53ms     4.34ms   371.18ms
    HTTP codes:
      1xx - 0, 2xx - 91152, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8848
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8848
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
    Reqs/sec      7309.45    6364.83   70450.17
    Latency        6.83ms     3.83ms   352.13ms
    HTTP codes:
      1xx - 0, 2xx - 89282, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10718
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10718
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
    Reqs/sec     24225.73    7575.67   36160.24
    Latency        2.06ms     2.05ms   184.97ms
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
    Reqs/sec     21368.31    5769.40   30336.89
    Latency        2.34ms     2.02ms   179.83ms
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
    Reqs/sec     64003.05    3431.14   70162.21
    Latency      780.04us    72.59us     3.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.08MB/s
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
    Reqs/sec     19082.68    8069.24   69139.00
    Latency        2.61ms     2.33ms   204.93ms
    HTTP codes:
      1xx - 0, 2xx - 93098, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6902
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6902
    Throughput:     4.03MB/s
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
    Reqs/sec     26394.05    9232.63   80738.68
    Latency        1.89ms     1.85ms   161.48ms
    HTTP codes:
      1xx - 0, 2xx - 95631, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4369
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4369
    Throughput:     5.78MB/s
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
    Reqs/sec     75741.90    2884.85   83617.95
    Latency      657.08us   151.92us     7.71ms
    HTTP codes:
      1xx - 0, 2xx - 96831, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3169
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3169
    Throughput:    11.61MB/s
  ```


