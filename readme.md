## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76434` | `1993` | `83697` |
| **81%** | [Hyper Express](#hyper-express) | `61726` | `3495` | `65550` |
| **35%** | [Node (Default)](#node-default) | `26670` | `8344` | `65776` |
| **32%** | [Fastify](#fastify) | `24170` | `7376` | `36682` |
| **28%** | [Hono](#hono) | `21490` | `5768` | `30314` |
| **26%** | [Koa](#koa) | `19545` | `9032` | `78164` |
| **11%** | [Carbon](#carbon) | `8349` | `1498` | `10603` |
| **9%** | [Express](#express) | `6516` | `1084` | `8552` |


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
    Reqs/sec      8991.30    5109.03   61800.64
    Latency        5.55ms     4.33ms   372.44ms
    HTTP codes:
      1xx - 0, 2xx - 93532, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6468
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6468
    Throughput:     1.91MB/s
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
    Reqs/sec      7013.67    5381.94   64166.80
    Latency        7.10ms     3.81ms   353.11ms
    HTTP codes:
      1xx - 0, 2xx - 91195, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8805
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8805
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
    Reqs/sec     24934.41    7970.10   36068.35
    Latency        2.00ms     2.06ms   183.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.66MB/s
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
    Reqs/sec     21847.20    6250.26   30850.75
    Latency        2.29ms     2.08ms   187.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.93MB/s
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
    Reqs/sec     63760.62    3462.54   66081.79
    Latency      782.05us    62.83us     2.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.06MB/s
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
    Reqs/sec     19234.20    7808.46   67106.96
    Latency        2.59ms     2.40ms   210.45ms
    HTTP codes:
      1xx - 0, 2xx - 93289, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6711
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6711
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
    Reqs/sec     26772.81    9104.14   76179.50
    Latency        1.86ms     1.90ms   164.73ms
    HTTP codes:
      1xx - 0, 2xx - 95794, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4206
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4206
    Throughput:     5.87MB/s
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
    Reqs/sec     75565.68    2445.90   79876.62
    Latency      658.59us   108.32us     5.13ms
    HTTP codes:
      1xx - 0, 2xx - 97393, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2607
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2607
    Throughput:    11.66MB/s
  ```


