## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72255` | `4224` | `91110` |
| **80%** | [Hyper Express](#hyper-express) | `57504` | `5128` | `69997` |
| **30%** | [Fastify](#fastify) | `21365` | `5963` | `37009` |
| **29%** | [Hono](#hono) | `21080` | `6454` | `30688` |
| **29%** | [Node (Default)](#node-default) | `20986` | `5251` | `63262` |
| **27%** | [Koa](#koa) | `19589` | `8648` | `64158` |
| **11%** | [Carbon](#carbon) | `7650` | `1402` | `10698` |
| **9%** | [Express](#express) | `6210` | `1122` | `8331` |


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
    Reqs/sec      7923.08    5225.62   68149.44
    Latency        6.30ms     4.52ms   386.35ms
    HTTP codes:
      1xx - 0, 2xx - 92524, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7476
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7476
    Throughput:     1.67MB/s
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
    Reqs/sec      6113.90    1060.81    8304.64
    Latency        8.17ms     3.89ms   371.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     21026.30    6182.25   36084.86
    Latency        2.38ms     2.04ms   184.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     21147.08    6411.48   30677.55
    Latency        2.36ms     2.13ms   184.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     59790.73    3364.77   64439.75
    Latency      834.15us    83.63us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.49MB/s
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
    Reqs/sec     18557.61    7847.30   57857.85
    Latency        2.69ms     2.41ms   210.66ms
    HTTP codes:
      1xx - 0, 2xx - 93847, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6153
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6153
    Throughput:     3.93MB/s
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
    Reqs/sec     20992.61    6110.58   70857.70
    Latency        2.37ms     2.07ms   176.30ms
    HTTP codes:
      1xx - 0, 2xx - 96138, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3862
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3862
    Throughput:     4.62MB/s
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
    Reqs/sec     71426.22    4124.47   85589.93
    Latency      697.33us   172.63us     8.97ms
    HTTP codes:
      1xx - 0, 2xx - 96963, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3037
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3037
    Throughput:    10.96MB/s
  ```


