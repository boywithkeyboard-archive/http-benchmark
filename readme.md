## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73235` | `2531` | `80680` |
| **84%** | [Hyper Express](#hyper-express) | `61738` | `6601` | `78329` |
| **33%** | [Node (Default)](#node-default) | `24196` | `7834` | `73402` |
| **32%** | [Fastify](#fastify) | `23542` | `8184` | `36275` |
| **27%** | [Hono](#hono) | `20099` | `5919` | `29683` |
| **25%** | [Koa](#koa) | `18524` | `9411` | `79553` |
| **11%** | [Carbon](#carbon) | `7972` | `1476` | `10293` |
| **9%** | [Express](#express) | `6358` | `1113` | `8411` |


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
    Reqs/sec      8499.55    6262.90   81589.24
    Latency        5.86ms     4.44ms   380.87ms
    HTTP codes:
      1xx - 0, 2xx - 91001, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8999
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8999
    Throughput:     1.76MB/s
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
    Reqs/sec      6171.89    1050.23    8275.57
    Latency        8.10ms     3.65ms   351.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23069.36    7496.21   37682.29
    Latency        2.17ms     2.01ms   183.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.22MB/s
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
    Reqs/sec     21214.21    6509.90   30601.05
    Latency        2.35ms     2.03ms   181.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     59809.32    3184.62   63115.63
    Latency      833.56us    86.36us     4.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.50MB/s
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
    Reqs/sec     17976.20    8449.23   66197.10
    Latency        2.78ms     2.36ms   211.51ms
    HTTP codes:
      1xx - 0, 2xx - 92143, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7857
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7857
    Throughput:     3.75MB/s
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
    Reqs/sec     23626.24    7487.72   77252.86
    Latency        2.11ms     2.02ms   173.34ms
    HTTP codes:
      1xx - 0, 2xx - 96280, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3720
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3720
    Throughput:     5.22MB/s
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
    Reqs/sec     73416.56    2426.69   80675.46
    Latency      678.01us   144.12us     5.12ms
    HTTP codes:
      1xx - 0, 2xx - 97257, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2743
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2743
    Throughput:    11.31MB/s
  ```


