## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76395` | `3262` | `85616` |
| **84%** | [Hyper Express](#hyper-express) | `63929` | `4111` | `68075` |
| **36%** | [Node (Default)](#node-default) | `27237` | `9737` | `85455` |
| **31%** | [Fastify](#fastify) | `24054` | `7017` | `36203` |
| **27%** | [Hono](#hono) | `20699` | `5899` | `29847` |
| **25%** | [Koa](#koa) | `18783` | `8359` | `67974` |
| **11%** | [Carbon](#carbon) | `8362` | `1478` | `10362` |
| **8%** | [Express](#express) | `6436` | `1069` | `8316` |


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
    Reqs/sec      9171.61    6435.33   76622.93
    Latency        5.44ms     4.26ms   369.47ms
    HTTP codes:
      1xx - 0, 2xx - 91186, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8814
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8814
    Throughput:     1.90MB/s
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
    Reqs/sec      7161.72    6662.61   76084.35
    Latency        6.97ms     3.97ms   364.30ms
    HTTP codes:
      1xx - 0, 2xx - 88396, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11604
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11604
    Throughput:     1.81MB/s
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
    Reqs/sec     24450.50    7614.28   37191.36
    Latency        2.04ms     1.99ms   181.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.55MB/s
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
    Reqs/sec     20975.00    5597.46   30583.50
    Latency        2.38ms     2.03ms   185.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     63356.16    3620.69   65761.78
    Latency      786.98us    72.06us     3.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     20018.88    8711.36   68292.69
    Latency        2.49ms     2.37ms   208.02ms
    HTTP codes:
      1xx - 0, 2xx - 91830, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8170
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8170
    Throughput:     4.16MB/s
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
    Reqs/sec     26918.23    8348.26   65095.01
    Latency        1.85ms     1.84ms   155.97ms
    HTTP codes:
      1xx - 0, 2xx - 96876, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3124
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3124
    Throughput:     5.98MB/s
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
    Reqs/sec     75824.31    3215.75   86862.15
    Latency      658.44us   141.41us     7.55ms
    HTTP codes:
      1xx - 0, 2xx - 97540, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2460
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2460
    Throughput:    11.68MB/s
  ```


