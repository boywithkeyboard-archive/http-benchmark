## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73368` | `4156` | `80601` |
| **86%** | [Hyper Express](#hyper-express) | `62943` | `2835` | `68123` |
| **36%** | [Node (Default)](#node-default) | `26537` | `8479` | `57547` |
| **34%** | [Fastify](#fastify) | `25068` | `7420` | `36179` |
| **28%** | [Hono](#hono) | `20828` | `5629` | `29975` |
| **25%** | [Koa](#koa) | `18486` | `7325` | `60438` |
| **11%** | [Carbon](#carbon) | `8234` | `1370` | `10419` |
| **9%** | [Express](#express) | `6438` | `1024` | `8421` |


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
    Reqs/sec      8770.85    3737.10   52153.33
    Latency        5.65ms     4.30ms   369.98ms
    HTTP codes:
      1xx - 0, 2xx - 94262, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5738
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5738
    Throughput:     1.89MB/s
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
    Reqs/sec      6967.45    4090.42   51790.22
    Latency        7.17ms     3.97ms   365.86ms
    HTTP codes:
      1xx - 0, 2xx - 93103, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6897
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6897
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
    Reqs/sec     24340.66    7128.14   36794.02
    Latency        2.05ms     1.95ms   175.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     20647.81    6074.47   30872.50
    Latency        2.42ms     2.05ms   184.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     62311.86    3852.21   72964.50
    Latency      799.32us    66.59us     3.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.86MB/s
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
    Reqs/sec     19338.35    6848.55   53854.17
    Latency        2.58ms     2.43ms   209.37ms
    HTTP codes:
      1xx - 0, 2xx - 94190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5810
    Throughput:     4.12MB/s
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
    Reqs/sec     26380.75    8007.03   59110.15
    Latency        1.89ms     1.83ms   159.05ms
    HTTP codes:
      1xx - 0, 2xx - 96639, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3361
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3361
    Throughput:     5.84MB/s
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
    Reqs/sec     74567.55    5655.22   89765.52
    Latency      668.96us   172.94us     9.24ms
    HTTP codes:
      1xx - 0, 2xx - 97661, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2339
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2339
    Throughput:    11.51MB/s
  ```


