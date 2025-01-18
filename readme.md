## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `77717` | `10501` | `143595` |
| **83%** | [Hyper Express](#hyper-express) | `64683` | `3930` | `68772` |
| **35%** | [Node (Default)](#node-default) | `27252` | `7921` | `51931` |
| **31%** | [Fastify](#fastify) | `24177` | `6669` | `37156` |
| **28%** | [Hono](#hono) | `21422` | `5734` | `30287` |
| **23%** | [Koa](#koa) | `18040` | `6248` | `49784` |
| **11%** | [Carbon](#carbon) | `8198` | `1364` | `10250` |
| **8%** | [Express](#express) | `6462` | `1037` | `8420` |


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
    Reqs/sec      8703.68    4731.53   61343.74
    Latency        5.74ms     4.29ms   373.20ms
    HTTP codes:
      1xx - 0, 2xx - 93224, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6776
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6776
    Throughput:     1.84MB/s
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
    Reqs/sec      6472.77    1047.99    8348.49
    Latency        7.72ms     3.61ms   343.74ms
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
    Reqs/sec     23500.18    7244.28   36710.27
    Latency        2.13ms     2.07ms   185.33ms
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
    Reqs/sec     20712.23    5969.30   30139.59
    Latency        2.41ms     2.06ms   186.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     66301.12    3586.19   72438.84
    Latency      751.78us    63.36us     3.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.42MB/s
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
    Reqs/sec     19185.27    7091.46   57824.89
    Latency        2.60ms     2.38ms   210.20ms
    HTTP codes:
      1xx - 0, 2xx - 93560, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6440
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6440
    Throughput:     4.06MB/s
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
    Reqs/sec     27523.66    8532.14   49243.72
    Latency        1.81ms     1.83ms   161.23ms
    HTTP codes:
      1xx - 0, 2xx - 97921, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2079
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2079
    Throughput:     6.17MB/s
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
    Reqs/sec     75054.02    5883.40   85835.16
    Latency      664.11us   166.75us     9.82ms
    HTTP codes:
      1xx - 0, 2xx - 96924, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3076
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3076
    Throughput:    11.50MB/s
  ```


