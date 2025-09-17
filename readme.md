## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74299` | `2899` | `80200` |
| **82%** | [Hyper Express](#hyper-express) | `61210` | `3512` | `64209` |
| **36%** | [Node (Default)](#node-default) | `26867` | `9848` | `77077` |
| **32%** | [Fastify](#fastify) | `23725` | `7146` | `35989` |
| **29%** | [Hono](#hono) | `21361` | `6332` | `30444` |
| **26%** | [Koa](#koa) | `19009` | `8857` | `81155` |
| **11%** | [Carbon](#carbon) | `8277` | `1452` | `10444` |
| **9%** | [Express](#express) | `6460` | `1072` | `8393` |


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
    Reqs/sec      9012.65    5575.51   66621.54
    Latency        5.53ms     4.23ms   365.66ms
    HTTP codes:
      1xx - 0, 2xx - 92186, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7814
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7814
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
    Reqs/sec      7065.05    5534.39   66852.32
    Latency        7.07ms     3.89ms   356.99ms
    HTTP codes:
      1xx - 0, 2xx - 90897, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9103
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9103
    Throughput:     1.84MB/s
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
    Reqs/sec     24787.88    7742.90   36876.75
    Latency        2.02ms     2.06ms   182.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     20754.14    5854.18   30419.65
    Latency        2.41ms     1.98ms   177.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     62154.26    3590.85   65869.98
    Latency      801.85us    67.96us     2.75ms
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
    Reqs/sec     18309.34    7493.71   66409.63
    Latency        2.73ms     2.32ms   204.10ms
    HTTP codes:
      1xx - 0, 2xx - 93330, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6670
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6670
    Throughput:     3.86MB/s
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
    Reqs/sec     26171.09    8185.94   64016.39
    Latency        1.91ms     1.91ms   164.88ms
    HTTP codes:
      1xx - 0, 2xx - 97414, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2586
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2586
    Throughput:     5.84MB/s
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
    Reqs/sec     74909.11    2889.61   83083.64
    Latency      664.53us   172.12us     8.15ms
    HTTP codes:
      1xx - 0, 2xx - 96616, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3384
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3384
    Throughput:    11.46MB/s
  ```


