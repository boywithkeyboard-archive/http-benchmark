## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `195999` | `9271` | `204790` |
| **87%** | [Hyper Express](#hyper-express) | `169745` | `11174` | `177344` |
| **38%** | [Node (Default)](#node-default) | `75280` | `17143` | `161456` |
| **37%** | [Fastify](#fastify) | `71886` | `15084` | `91134` |
| **35%** | [Hono](#hono) | `67861` | `15628` | `84015` |
| **34%** | [Koa](#koa) | `67116` | `23490` | `173652` |
| **15%** | [Carbon](#carbon) | `29974` | `6745` | `36215` |
| **10%** | [Express](#express) | `19241` | `3168` | `23511` |


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
    Reqs/sec     31996.25   18247.02  161223.83
    Latency        1.56ms     2.45ms   199.62ms
    HTTP codes:
      1xx - 0, 2xx - 89742, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10258
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10258
    Throughput:     6.53MB/s
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
    Reqs/sec     22407.04   22009.14  176757.19
    Latency        2.22ms     2.01ms   175.16ms
    HTTP codes:
      1xx - 0, 2xx - 82964, 3xx - 0, 4xx - 0, 5xx - 0
      others - 17036
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 17036
    Throughput:     5.34MB/s
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
    Reqs/sec     81694.75   30229.24  169924.93
    Latency      607.28us   830.72us    52.28ms
    HTTP codes:
      1xx - 0, 2xx - 76659, 3xx - 0, 4xx - 0, 5xx - 0
      others - 23341
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 23341
    Throughput:    14.23MB/s
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
    Reqs/sec     72060.93   29317.97  185931.36
    Latency      691.43us   775.27us    53.66ms
    HTTP codes:
      1xx - 0, 2xx - 84977, 3xx - 0, 4xx - 0, 5xx - 0
      others - 15023
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 15023
    Throughput:    13.85MB/s
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
    Reqs/sec    163331.53   14340.65  177838.33
    Latency      303.99us   166.11us     6.32ms
    HTTP codes:
      1xx - 0, 2xx - 90725, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9275
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9275
    Throughput:    21.03MB/s
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
    Reqs/sec     68198.39   24692.77  172420.22
    Latency      730.66us   816.34us    50.67ms
    HTTP codes:
      1xx - 0, 2xx - 88866, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11134
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11134
    Throughput:    13.70MB/s
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
    Reqs/sec     76868.08   22817.62  173619.97
    Latency      648.18us   672.81us    47.95ms
    HTTP codes:
      1xx - 0, 2xx - 92942, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7058
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7058
    Throughput:    16.36MB/s
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
    Reqs/sec    189342.05    9990.78  201457.91
    Latency      262.19us   138.96us     5.24ms
    HTTP codes:
      1xx - 0, 2xx - 95212, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4788
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4788
    Throughput:    28.49MB/s
  ```


