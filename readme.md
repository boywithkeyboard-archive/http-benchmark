## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70582` | `5032` | `86781` |
| **84%** | [Hyper Express](#hyper-express) | `59097` | `3660` | `66697` |
| **31%** | [Fastify](#fastify) | `21709` | `6093` | `35348` |
| **30%** | [Hono](#hono) | `21082` | `6615` | `31811` |
| **29%** | [Node (Default)](#node-default) | `20493` | `4832` | `63309` |
| **27%** | [Koa](#koa) | `19022` | `8247` | `64006` |
| **11%** | [Carbon](#carbon) | `7747` | `1419` | `10601` |
| **9%** | [Express](#express) | `6349` | `1118` | `8483` |


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
    Reqs/sec      8201.37    5343.45   66691.57
    Latency        6.09ms     4.52ms   382.97ms
    HTTP codes:
      1xx - 0, 2xx - 92121, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7879
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7879
    Throughput:     1.72MB/s
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
    Reqs/sec      6250.21    1093.65    8418.68
    Latency        8.00ms     3.71ms   358.30ms
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
    Reqs/sec     21617.57    6453.92   37080.28
    Latency        2.31ms     1.98ms   182.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.91MB/s
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
    Reqs/sec     21218.87    6776.21   30801.84
    Latency        2.35ms     2.32ms   205.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     60459.11    3327.70   65491.50
    Latency      825.03us   105.09us     5.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.59MB/s
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
    Reqs/sec     18069.55    7288.97   57875.82
    Latency        2.76ms     2.42ms   210.90ms
    HTTP codes:
      1xx - 0, 2xx - 94363, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5637
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5637
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
    Reqs/sec     20217.83    4907.45   58419.51
    Latency        2.46ms     2.01ms   175.47ms
    HTTP codes:
      1xx - 0, 2xx - 97399, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2601
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2601
    Throughput:     4.52MB/s
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
    Reqs/sec     71915.27    5201.21   90368.44
    Latency      692.26us   196.85us     7.59ms
    HTTP codes:
      1xx - 0, 2xx - 97179, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2821
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2821
    Throughput:    11.06MB/s
  ```


