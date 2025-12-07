## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74456` | `2596` | `80797` |
| **87%** | [Hyper Express](#hyper-express) | `64954` | `4120` | `68216` |
| **34%** | [Node (Default)](#node-default) | `25524` | `7833` | `62950` |
| **32%** | [Fastify](#fastify) | `23928` | `6980` | `36193` |
| **27%** | [Koa](#koa) | `20440` | `9389` | `78118` |
| **27%** | [Hono](#hono) | `20260` | `5350` | `29646` |
| **11%** | [Carbon](#carbon) | `8294` | `1449` | `10468` |
| **9%** | [Express](#express) | `6537` | `1122` | `8448` |


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
    Reqs/sec      9195.82    5779.77   68951.91
    Latency        5.43ms     4.24ms   365.93ms
    HTTP codes:
      1xx - 0, 2xx - 92069, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7931
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7931
    Throughput:     1.92MB/s
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
    Reqs/sec      6998.84    5251.76   65601.16
    Latency        7.13ms     4.00ms   366.62ms
    HTTP codes:
      1xx - 0, 2xx - 91333, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8667
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8667
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
    Reqs/sec     24532.54    7575.78   35825.10
    Latency        2.04ms     2.02ms   182.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     21106.28    5699.38   29520.10
    Latency        2.37ms     2.02ms   184.28ms
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
    Reqs/sec     63464.06    3608.24   67927.23
    Latency      785.55us    64.19us     2.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.02MB/s
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
    Reqs/sec     19905.45    9984.59   77897.24
    Latency        2.51ms     2.33ms   206.15ms
    HTTP codes:
      1xx - 0, 2xx - 90216, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9784
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9784
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
    Reqs/sec     26788.71    9170.34   74172.39
    Latency        1.86ms     1.97ms   168.00ms
    HTTP codes:
      1xx - 0, 2xx - 95897, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4103
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4103
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
    Reqs/sec     75196.26    2736.50   84402.43
    Latency      661.33us   176.64us    10.96ms
    HTTP codes:
      1xx - 0, 2xx - 96513, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3487
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3487
    Throughput:    11.49MB/s
  ```


