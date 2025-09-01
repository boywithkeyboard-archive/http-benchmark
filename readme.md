## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76241` | `5145` | `111276` |
| **83%** | [Hyper Express](#hyper-express) | `62955` | `3788` | `66189` |
| **34%** | [Node (Default)](#node-default) | `26087` | `8023` | `60764` |
| **32%** | [Fastify](#fastify) | `24720` | `7954` | `36977` |
| **28%** | [Hono](#hono) | `21390` | `6071` | `29689` |
| **25%** | [Koa](#koa) | `18827` | `8055` | `66927` |
| **11%** | [Carbon](#carbon) | `8211` | `1430` | `10317` |
| **8%** | [Express](#express) | `6421` | `1035` | `8495` |


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
    Reqs/sec      9119.61    6999.55   75067.89
    Latency        5.47ms     4.27ms   371.00ms
    HTTP codes:
      1xx - 0, 2xx - 89616, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10384
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10384
    Throughput:     1.86MB/s
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
    Reqs/sec      7150.25    5883.32   78695.79
    Latency        6.98ms     3.88ms   354.68ms
    HTTP codes:
      1xx - 0, 2xx - 90371, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9629
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9629
    Throughput:     1.85MB/s
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
    Reqs/sec     23714.86    7303.32   36263.92
    Latency        2.11ms     2.04ms   181.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     21324.63    5876.77   30339.07
    Latency        2.34ms     2.07ms   182.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     63938.44    3535.37   66397.69
    Latency      780.08us    66.65us     2.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.08MB/s
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
    Reqs/sec     18573.02    7708.44   68573.90
    Latency        2.69ms     2.37ms   206.48ms
    HTTP codes:
      1xx - 0, 2xx - 93294, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6706
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6706
    Throughput:     3.91MB/s
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
    Reqs/sec     25773.35    8352.87   76407.87
    Latency        1.94ms     1.95ms   168.61ms
    HTTP codes:
      1xx - 0, 2xx - 96192, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3808
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3808
    Throughput:     5.68MB/s
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
    Reqs/sec     75264.45    3048.28   81096.71
    Latency      662.11us   135.34us     5.35ms
    HTTP codes:
      1xx - 0, 2xx - 96941, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3059
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3059
    Throughput:    11.55MB/s
  ```


