## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74996` | `2310` | `78912` |
| **85%** | [Hyper Express](#hyper-express) | `63753` | `3622` | `67331` |
| **33%** | [Node (Default)](#node-default) | `24585` | `8952` | `81507` |
| **32%** | [Fastify](#fastify) | `24327` | `7539` | `35781` |
| **28%** | [Hono](#hono) | `21019` | `5771` | `29888` |
| **24%** | [Koa](#koa) | `18332` | `7355` | `65703` |
| **11%** | [Carbon](#carbon) | `8089` | `1393` | `10204` |
| **9%** | [Express](#express) | `6552` | `1117` | `8489` |


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
    Reqs/sec      8703.26    5383.21   66802.56
    Latency        5.72ms     4.33ms   372.20ms
    HTTP codes:
      1xx - 0, 2xx - 92529, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7471
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7471
    Throughput:     1.83MB/s
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
    Reqs/sec      6478.70    1105.02    8333.27
    Latency        7.71ms     3.74ms   358.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23473.32    7118.69   35116.78
    Latency        2.13ms     2.09ms   185.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.33MB/s
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
    Reqs/sec     21094.12    5817.06   28622.49
    Latency        2.37ms     2.10ms   189.91ms
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
    Reqs/sec     63688.98    3714.45   67340.71
    Latency      783.11us    68.32us     2.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     18832.80    9082.73   78127.50
    Latency        2.65ms     2.49ms   219.64ms
    HTTP codes:
      1xx - 0, 2xx - 90986, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9014
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9014
    Throughput:     3.87MB/s
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
    Reqs/sec     25361.45    8178.64   77254.56
    Latency        1.96ms     2.07ms   175.14ms
    HTTP codes:
      1xx - 0, 2xx - 96621, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3379
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3379
    Throughput:     5.62MB/s
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
    Reqs/sec     75649.57    3083.62   88033.69
    Latency      658.61us   138.26us     8.06ms
    HTTP codes:
      1xx - 0, 2xx - 97439, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2561
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2561
    Throughput:    11.66MB/s
  ```


