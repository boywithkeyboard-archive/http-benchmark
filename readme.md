## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74918` | `2079` | `78806` |
| **81%** | [Hyper Express](#hyper-express) | `60679` | `4033` | `65899` |
| **34%** | [Node (Default)](#node-default) | `25277` | `8088` | `64387` |
| **32%** | [Fastify](#fastify) | `23788` | `7253` | `35088` |
| **27%** | [Hono](#hono) | `20264` | `5696` | `29657` |
| **25%** | [Koa](#koa) | `18534` | `8320` | `71768` |
| **11%** | [Carbon](#carbon) | `7940` | `1366` | `10337` |
| **8%** | [Express](#express) | `6212` | `1023` | `8193` |


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
    Reqs/sec      8922.29    6964.74   79197.37
    Latency        5.60ms     4.45ms   382.66ms
    HTTP codes:
      1xx - 0, 2xx - 89476, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10524
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10524
    Throughput:     1.81MB/s
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
    Reqs/sec      6799.83    5365.71   77108.68
    Latency        7.34ms     4.01ms   368.53ms
    HTTP codes:
      1xx - 0, 2xx - 91128, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8872
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8872
    Throughput:     1.77MB/s
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
    Reqs/sec     24337.69    7634.81   36022.31
    Latency        2.05ms     2.02ms   181.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.53MB/s
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
    Reqs/sec     20817.76    5713.90   28912.84
    Latency        2.40ms     2.08ms   190.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     63195.05    4200.57   68595.87
    Latency      789.39us    74.90us     3.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     19897.14    9145.22   79614.12
    Latency        2.51ms     2.47ms   218.35ms
    HTTP codes:
      1xx - 0, 2xx - 92116, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7884
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7884
    Throughput:     4.14MB/s
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
    Reqs/sec     25950.07    8592.40   70022.49
    Latency        1.92ms     1.87ms   159.07ms
    HTTP codes:
      1xx - 0, 2xx - 96100, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3900
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3900
    Throughput:     5.72MB/s
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
    Reqs/sec     74362.98    2989.86   80763.46
    Latency      670.56us   146.44us     6.96ms
    HTTP codes:
      1xx - 0, 2xx - 97507, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2493
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2493
    Throughput:    11.47MB/s
  ```


