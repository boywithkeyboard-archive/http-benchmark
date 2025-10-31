## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `129995` | `5842` | `149346` |
| **81%** | [Hyper Express](#hyper-express) | `105074` | `7283` | `109234` |
| **30%** | [Node (Default)](#node-default) | `39585` | `11694` | `105006` |
| **29%** | [Fastify](#fastify) | `37688` | `8815` | `56372` |
| **27%** | [Hono](#hono) | `34533` | `7537` | `49623` |
| **26%** | [Koa](#koa) | `33570` | `14728` | `113112` |
| **10%** | [Carbon](#carbon) | `12661` | `2735` | `16960` |
| **7%** | [Express](#express) | `9021` | `1666` | `12073` |


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
    Reqs/sec     14254.32   11838.46  126271.34
    Latency        3.49ms     3.64ms   310.11ms
    HTTP codes:
      1xx - 0, 2xx - 87925, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12075
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12075
    Throughput:     2.86MB/s
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
    Reqs/sec     10194.24    9550.40  105359.20
    Latency        4.89ms     3.21ms   292.74ms
    HTTP codes:
      1xx - 0, 2xx - 88602, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11398
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11398
    Throughput:     2.59MB/s
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
    Reqs/sec     37687.33    8860.20   57354.81
    Latency        1.33ms     1.38ms   121.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.55MB/s
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
    Reqs/sec     33143.35    7696.26   50222.04
    Latency        1.51ms     1.43ms   124.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.49MB/s
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
    Reqs/sec    104717.03    7319.18  107968.18
    Latency      475.80us    45.83us     2.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.87MB/s
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
    Reqs/sec     33207.69   17106.81  129117.56
    Latency        1.50ms     1.72ms   148.00ms
    HTTP codes:
      1xx - 0, 2xx - 87223, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12777
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12777
    Throughput:     6.57MB/s
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
    Reqs/sec     39047.78   12514.52  124412.84
    Latency        1.28ms     1.29ms   111.27ms
    HTTP codes:
      1xx - 0, 2xx - 95025, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4975
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4975
    Throughput:     8.50MB/s
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
    Reqs/sec    128558.67    6153.39  138073.71
    Latency      386.27us   129.49us     9.05ms
    HTTP codes:
      1xx - 0, 2xx - 95753, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4247
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4247
    Throughput:    19.48MB/s
  ```


