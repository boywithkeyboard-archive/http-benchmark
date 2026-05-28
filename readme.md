## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71621` | `4559` | `85052` |
| **82%** | [Hyper Express](#hyper-express) | `58691` | `3490` | `65299` |
| **30%** | [Hono](#hono) | `21608` | `6563` | `30132` |
| **28%** | [Node (Default)](#node-default) | `20178` | `6493` | `78970` |
| **28%** | [Fastify](#fastify) | `19796` | `4449` | `34661` |
| **27%** | [Koa](#koa) | `19059` | `10217` | `79038` |
| **10%** | [Carbon](#carbon) | `7444` | `1309` | `10385` |
| **9%** | [Express](#express) | `6180` | `1105` | `8243` |


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
    Reqs/sec      8051.91    6714.47   75595.28
    Latency        6.20ms     4.49ms   378.95ms
    HTTP codes:
      1xx - 0, 2xx - 89064, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10936
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10936
    Throughput:     1.63MB/s
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
    Reqs/sec      6348.45    1171.58    8298.82
    Latency        7.87ms     3.85ms   366.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     20550.43    5199.79   36115.95
    Latency        2.43ms     2.12ms   189.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     20306.87    5689.70   29046.11
    Latency        2.46ms     2.09ms   183.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     59208.71    3465.45   64031.82
    Latency      842.42us    92.73us     4.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.41MB/s
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
    Reqs/sec     18626.59    8913.93   77167.83
    Latency        2.68ms     2.49ms   213.28ms
    HTTP codes:
      1xx - 0, 2xx - 91584, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8416
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8416
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
    Reqs/sec     20388.53    5181.53   64389.51
    Latency        2.45ms     2.09ms   178.47ms
    HTTP codes:
      1xx - 0, 2xx - 96899, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3101
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3101
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
    Reqs/sec     73601.76    5912.45   90188.30
    Latency      677.09us   164.46us     6.81ms
    HTTP codes:
      1xx - 0, 2xx - 96814, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3186
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3186
    Throughput:    11.28MB/s
  ```


