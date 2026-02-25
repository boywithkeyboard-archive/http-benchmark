## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75754` | `2789` | `83887` |
| **84%** | [Hyper Express](#hyper-express) | `63397` | `3340` | `67399` |
| **36%** | [Node (Default)](#node-default) | `26948` | `8964` | `79957` |
| **32%** | [Fastify](#fastify) | `24006` | `7330` | `36491` |
| **29%** | [Hono](#hono) | `22248` | `6359` | `30939` |
| **26%** | [Koa](#koa) | `19371` | `8213` | `71422` |
| **11%** | [Carbon](#carbon) | `8408` | `1442` | `10525` |
| **9%** | [Express](#express) | `6483` | `1063` | `8472` |


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
    Reqs/sec      9392.08    7841.59   82199.36
    Latency        5.32ms     4.27ms   366.81ms
    HTTP codes:
      1xx - 0, 2xx - 87977, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12023
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12023
    Throughput:     1.88MB/s
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
    Reqs/sec      7101.05    5849.25   75771.65
    Latency        7.04ms     3.85ms   353.26ms
    HTTP codes:
      1xx - 0, 2xx - 90491, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9509
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9509
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
    Reqs/sec     24222.20    7067.18   36656.87
    Latency        2.06ms     1.94ms   175.66ms
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
    Reqs/sec     21278.71    6210.06   31033.91
    Latency        2.35ms     2.02ms   181.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.81MB/s
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
    Reqs/sec     64079.55    3433.01   70176.80
    Latency      778.99us    69.39us     3.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.09MB/s
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
    Reqs/sec     19324.96    9201.59   75222.70
    Latency        2.58ms     2.34ms   205.16ms
    HTTP codes:
      1xx - 0, 2xx - 91226, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8774
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8774
    Throughput:     3.99MB/s
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
    Reqs/sec     27376.63    8865.21   81088.69
    Latency        1.82ms     1.87ms   159.93ms
    HTTP codes:
      1xx - 0, 2xx - 96562, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3438
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3438
    Throughput:     6.05MB/s
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
    Reqs/sec     76260.43    3574.07   87414.27
    Latency      652.58us   165.94us     9.44ms
    HTTP codes:
      1xx - 0, 2xx - 96434, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3566
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3566
    Throughput:    11.63MB/s
  ```


