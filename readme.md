## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `122036` | `7217` | `135519` |
| **81%** | [Hyper Express](#hyper-express) | `98511` | `7312` | `103550` |
| **31%** | [Node (Default)](#node-default) | `37255` | `12827` | `127241` |
| **27%** | [Koa](#koa) | `33546` | `17694` | `127166` |
| **27%** | [Fastify](#fastify) | `33158` | `7716` | `53308` |
| **26%** | [Hono](#hono) | `31725` | `7884` | `48913` |
| **10%** | [Carbon](#carbon) | `12272` | `2502` | `16691` |
| **7%** | [Express](#express) | `8754` | `1763` | `11927` |


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
    Reqs/sec     13488.90   11284.55  125686.31
    Latency        3.70ms     3.64ms   313.79ms
    HTTP codes:
      1xx - 0, 2xx - 88311, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11689
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11689
    Throughput:     2.71MB/s
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
    Reqs/sec     10135.98   11416.57  119294.00
    Latency        4.91ms     3.39ms   306.85ms
    HTTP codes:
      1xx - 0, 2xx - 85033, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14967
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14967
    Throughput:     2.47MB/s
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
    Reqs/sec     35305.67    8889.70   56045.78
    Latency        1.42ms     1.43ms   127.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.01MB/s
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
    Reqs/sec     32377.26    8089.27   48010.18
    Latency        1.54ms     1.47ms   128.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.31MB/s
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
    Reqs/sec     98111.30    7346.85  103173.36
    Latency      507.67us    55.34us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    13.94MB/s
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
    Reqs/sec     31547.73   14240.30  107008.30
    Latency        1.58ms     1.74ms   148.32ms
    HTTP codes:
      1xx - 0, 2xx - 89661, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10339
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10339
    Throughput:     6.40MB/s
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
    Reqs/sec     38659.17   12542.36  104332.71
    Latency        1.29ms     1.40ms   116.61ms
    HTTP codes:
      1xx - 0, 2xx - 94168, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5832
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5832
    Throughput:     8.35MB/s
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
    Reqs/sec    123667.31    6940.84  137306.79
    Latency      401.59us   152.18us     9.24ms
    HTTP codes:
      1xx - 0, 2xx - 95905, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4095
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4095
    Throughput:    18.76MB/s
  ```


