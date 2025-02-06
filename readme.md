## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72632` | `4822` | `78637` |
| **86%** | [Hyper Express](#hyper-express) | `62316` | `3859` | `69488` |
| **36%** | [Node (Default)](#node-default) | `26206` | `7457` | `57680` |
| **34%** | [Fastify](#fastify) | `24788` | `7630` | `37521` |
| **30%** | [Hono](#hono) | `21950` | `6293` | `30648` |
| **25%** | [Koa](#koa) | `18507` | `6669` | `57678` |
| **11%** | [Carbon](#carbon) | `8133` | `1369` | `10410` |
| **9%** | [Express](#express) | `6511` | `1059` | `8454` |


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
    Reqs/sec      8864.36    4558.26   55454.04
    Latency        5.63ms     4.21ms   364.76ms
    HTTP codes:
      1xx - 0, 2xx - 93656, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6344
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6344
    Throughput:     1.88MB/s
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
    Reqs/sec      6394.47     994.02    8475.24
    Latency        7.82ms     3.63ms   349.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     23872.39    6928.68   36208.52
    Latency        2.09ms     2.07ms   183.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     20862.92    5734.85   30004.24
    Latency        2.39ms     1.95ms   175.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     62838.84    3484.19   70777.54
    Latency      793.77us    76.29us     3.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     19179.91    6925.21   62767.43
    Latency        2.60ms     2.35ms   203.63ms
    HTTP codes:
      1xx - 0, 2xx - 93436, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6564
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6564
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
    Reqs/sec     26392.52    7620.74   54798.22
    Latency        1.89ms     1.81ms   155.16ms
    HTTP codes:
      1xx - 0, 2xx - 97284, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2716
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2716
    Throughput:     5.88MB/s
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
    Reqs/sec     74303.75    5343.64   89002.70
    Latency      670.85us   177.67us     9.27ms
    HTTP codes:
      1xx - 0, 2xx - 96879, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3121
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3121
    Throughput:    11.38MB/s
  ```


