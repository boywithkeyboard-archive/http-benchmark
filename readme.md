## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `67814` | `6733` | `83671` |
| **87%** | [Hyper Express](#hyper-express) | `58832` | `3599` | `64413` |
| **32%** | [Hono](#hono) | `22033` | `6894` | `30638` |
| **29%** | [Fastify](#fastify) | `19745` | `5420` | `35769` |
| **29%** | [Node (Default)](#node-default) | `19699` | `4722` | `62246` |
| **28%** | [Koa](#koa) | `18756` | `8230` | `67799` |
| **11%** | [Carbon](#carbon) | `7477` | `1426` | `10340` |
| **9%** | [Express](#express) | `6024` | `1059` | `8240` |


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
    Reqs/sec      7929.82    5126.53   69612.50
    Latency        6.30ms     4.83ms   403.48ms
    HTTP codes:
      1xx - 0, 2xx - 92663, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7337
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7337
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
    Reqs/sec      6166.09    1114.04    8329.99
    Latency        8.11ms     3.90ms   373.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     19087.19    4204.57   35364.14
    Latency        2.62ms     2.06ms   185.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.33MB/s
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
    Reqs/sec     21556.37    6888.08   30858.57
    Latency        2.32ms     2.12ms   195.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     58343.06    3320.11   64138.07
    Latency        0.85ms    97.01us     4.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.29MB/s
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
    Reqs/sec     18591.85    8619.21   73930.04
    Latency        2.68ms     2.51ms   215.85ms
    HTTP codes:
      1xx - 0, 2xx - 91874, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8126
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8126
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
    Reqs/sec     19783.95    6251.80   77270.62
    Latency        2.52ms     1.94ms   169.67ms
    HTTP codes:
      1xx - 0, 2xx - 95691, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4309
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4309
    Throughput:     4.34MB/s
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
    Reqs/sec     69791.16    3826.58   78745.91
    Latency      713.33us   191.21us     8.60ms
    HTTP codes:
      1xx - 0, 2xx - 95811, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4189
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4189
    Throughput:    10.59MB/s
  ```


