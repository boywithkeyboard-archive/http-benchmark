## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69815` | `4084` | `81502` |
| **82%** | [Hyper Express](#hyper-express) | `57497` | `3504` | `63458` |
| **28%** | [Fastify](#fastify) | `19729` | `6004` | `35114` |
| **27%** | [Node (Default)](#node-default) | `19034` | `4381` | `59063` |
| **24%** | [Koa](#koa) | `16753` | `7663` | `66093` |
| **23%** | [Hono](#hono) | `16309` | `3694` | `28986` |
| **10%** | [Carbon](#carbon) | `7210` | `1252` | `10129` |
| **8%** | [Express](#express) | `5883` | `1047` | `7990` |


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
    Reqs/sec      8160.07    6696.25   71070.07
    Latency        6.12ms     4.57ms   384.58ms
    HTTP codes:
      1xx - 0, 2xx - 89108, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10892
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10892
    Throughput:     1.65MB/s
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
    Reqs/sec      5996.03    1053.06    8148.96
    Latency        8.34ms     4.04ms   385.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.71MB/s
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
    Reqs/sec     18743.49    4383.72   34416.90
    Latency        2.66ms     2.15ms   195.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.25MB/s
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
    Reqs/sec     18340.01    4415.40   30076.49
    Latency        2.72ms     2.18ms   195.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.15MB/s
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
    Reqs/sec     58022.00    3408.34   65116.53
    Latency        0.86ms    85.18us     3.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.24MB/s
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
    Reqs/sec     15638.83    8083.68   73774.53
    Latency        3.19ms     2.50ms   215.83ms
    HTTP codes:
      1xx - 0, 2xx - 91045, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8955
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8955
    Throughput:     3.22MB/s
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
    Reqs/sec     18615.94    4513.43   61090.60
    Latency        2.68ms     2.10ms   182.11ms
    HTTP codes:
      1xx - 0, 2xx - 97195, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2805
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2805
    Throughput:     4.15MB/s
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
    Reqs/sec     70224.25    5108.78   86029.56
    Latency      709.72us   192.20us     8.25ms
    HTTP codes:
      1xx - 0, 2xx - 97447, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2553
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2553
    Throughput:    10.83MB/s
  ```


