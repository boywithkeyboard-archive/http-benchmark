## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73763` | `4714` | `85643` |
| **85%** | [Hyper Express](#hyper-express) | `63062` | `4411` | `68392` |
| **38%** | [Node (Default)](#node-default) | `28263` | `8648` | `48972` |
| **33%** | [Fastify](#fastify) | `24282` | `7097` | `36409` |
| **29%** | [Hono](#hono) | `21129` | `5965` | `30156` |
| **26%** | [Koa](#koa) | `19050` | `6739` | `52378` |
| **11%** | [Carbon](#carbon) | `8343` | `1433` | `10419` |
| **9%** | [Express](#express) | `6424` | `1051` | `8423` |


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
    Reqs/sec      8700.16    4314.44   59600.61
    Latency        5.73ms     4.22ms   366.47ms
    HTTP codes:
      1xx - 0, 2xx - 94058, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5942
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5942
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
    Reqs/sec      6915.67    4004.87   49730.92
    Latency        7.22ms     3.86ms   351.43ms
    HTTP codes:
      1xx - 0, 2xx - 93037, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6963
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6963
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
    Reqs/sec     24545.33    7299.99   38067.99
    Latency        2.03ms     1.96ms   174.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     21626.40    5983.71   29830.98
    Latency        2.31ms     1.97ms   174.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.88MB/s
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
    Reqs/sec     62213.44    2902.64   72835.73
    Latency      801.44us    71.38us     2.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18026.02    6142.39   46978.45
    Latency        2.76ms     2.42ms   208.05ms
    HTTP codes:
      1xx - 0, 2xx - 95256, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4744
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4744
    Throughput:     3.89MB/s
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
    Reqs/sec     25788.10    7038.08   47114.01
    Latency        1.94ms     1.83ms   159.16ms
    HTTP codes:
      1xx - 0, 2xx - 97866, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2134
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2134
    Throughput:     5.77MB/s
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
    Reqs/sec     74018.12    5340.80   80791.63
    Latency      672.37us   192.54us     9.96ms
    HTTP codes:
      1xx - 0, 2xx - 96751, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3249
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3249
    Throughput:    11.33MB/s
  ```


