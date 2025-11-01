## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75219` | `2354` | `83803` |
| **83%** | [Hyper Express](#hyper-express) | `62672` | `3770` | `66009` |
| **34%** | [Node (Default)](#node-default) | `25877` | `7986` | `68196` |
| **31%** | [Fastify](#fastify) | `23305` | `6954` | `36398` |
| **28%** | [Hono](#hono) | `20726` | `5957` | `30995` |
| **26%** | [Koa](#koa) | `19239` | `9165` | `72474` |
| **11%** | [Carbon](#carbon) | `8125` | `1436` | `10486` |
| **8%** | [Express](#express) | `6299` | `1006` | `8354` |


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
    Reqs/sec      8837.76    6117.23   69171.18
    Latency        5.65ms     4.43ms   383.70ms
    HTTP codes:
      1xx - 0, 2xx - 91156, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8844
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8844
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
    Reqs/sec      6831.33    5673.79   67451.94
    Latency        7.30ms     3.97ms   366.57ms
    HTTP codes:
      1xx - 0, 2xx - 90387, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9613
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9613
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
    Reqs/sec     23840.01    7006.93   36046.83
    Latency        2.09ms     1.96ms   179.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     20923.54    5749.51   30140.58
    Latency        2.39ms     2.04ms   185.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     62130.13    3344.21   65558.07
    Latency      802.35us    69.78us     2.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     19050.83    8908.81   74112.82
    Latency        2.62ms     2.42ms   212.12ms
    HTTP codes:
      1xx - 0, 2xx - 91147, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8853
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8853
    Throughput:     3.93MB/s
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
    Reqs/sec     26576.73    8311.69   58535.93
    Latency        1.88ms     1.92ms   164.94ms
    HTTP codes:
      1xx - 0, 2xx - 97648, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2352
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2352
    Throughput:     5.94MB/s
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
    Reqs/sec     73447.08    2047.05   79503.59
    Latency      678.85us   180.39us    11.01ms
    HTTP codes:
      1xx - 0, 2xx - 96307, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3693
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3693
    Throughput:    11.18MB/s
  ```


