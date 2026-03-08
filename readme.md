## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `124281` | `6570` | `134585` |
| **80%** | [Hyper Express](#hyper-express) | `99053` | `8875` | `104826` |
| **31%** | [Node (Default)](#node-default) | `39135` | `10401` | `97028` |
| **30%** | [Fastify](#fastify) | `37053` | `8504` | `54159` |
| **26%** | [Hono](#hono) | `32478` | `6892` | `47338` |
| **26%** | [Koa](#koa) | `32360` | `11695` | `87166` |
| **10%** | [Carbon](#carbon) | `11983` | `2416` | `16428` |
| **7%** | [Express](#express) | `8692` | `1702` | `11809` |


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
    Reqs/sec     13462.60   10847.32  111846.00
    Latency        3.70ms     3.66ms   318.30ms
    HTTP codes:
      1xx - 0, 2xx - 88434, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11566
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11566
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
    Reqs/sec     10203.50   10518.86  101894.24
    Latency        4.89ms     3.51ms   317.13ms
    HTTP codes:
      1xx - 0, 2xx - 86379, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13621
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13621
    Throughput:     2.53MB/s
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
    Reqs/sec     37355.71    8783.80   55158.82
    Latency        1.34ms     1.52ms   134.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.48MB/s
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
    Reqs/sec     31862.31    6712.16   46309.76
    Latency        1.57ms     1.49ms   130.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.19MB/s
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
    Reqs/sec     99483.41    6900.35  103663.39
    Latency      500.85us    48.50us     2.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.13MB/s
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
    Reqs/sec     32001.63   13994.11  104343.12
    Latency        1.56ms     1.78ms   153.27ms
    HTTP codes:
      1xx - 0, 2xx - 89887, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10113
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10113
    Throughput:     6.52MB/s
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
    Reqs/sec     38019.38   10086.63   98789.55
    Latency        1.31ms     1.40ms   114.82ms
    HTTP codes:
      1xx - 0, 2xx - 96277, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3723
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3723
    Throughput:     8.38MB/s
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
    Reqs/sec    120735.95    7386.81  140499.00
    Latency      411.24us   139.66us     7.64ms
    HTTP codes:
      1xx - 0, 2xx - 96333, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3667
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3667
    Throughput:    18.41MB/s
  ```


