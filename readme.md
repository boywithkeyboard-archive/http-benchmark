## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `102675` | `2643` | `113199` |
| **89%** | [Hyper Express](#hyper-express) | `90966` | `4661` | `102666` |
| **43%** | [Fastify](#fastify) | `44574` | `16142` | `66709` |
| **43%** | [Node (Default)](#node-default) | `44378` | `16547` | `113730` |
| **40%** | [Hono](#hono) | `40801` | `15654` | `58680` |
| **38%** | [Koa](#koa) | `38812` | `15597` | `94722` |
| **11%** | [Carbon](#carbon) | `11421` | `2367` | `16773` |
| **9%** | [Express](#express) | `9318` | `1910` | `14047` |


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
    Reqs/sec     13185.89    9779.31   98504.45
    Latency        3.78ms     3.47ms   295.34ms
    HTTP codes:
      1xx - 0, 2xx - 88822, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11178
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11178
    Throughput:     2.66MB/s
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
    Reqs/sec     10408.82    8496.40  104145.98
    Latency        4.80ms     2.97ms   273.37ms
    HTTP codes:
      1xx - 0, 2xx - 89576, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10424
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10424
    Throughput:     2.67MB/s
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
    Reqs/sec     46710.12   15512.44   65576.61
    Latency        1.07ms     1.44ms   128.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.60MB/s
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
    Reqs/sec     40766.38   14570.66   58339.26
    Latency        1.23ms     1.77ms   153.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.21MB/s
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
    Reqs/sec     89541.39    4315.30   94661.35
    Latency      556.88us    55.05us     2.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.72MB/s
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
    Reqs/sec     39705.18   15157.36   91719.33
    Latency        1.26ms     1.74ms   147.69ms
    HTTP codes:
      1xx - 0, 2xx - 92143, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7857
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7857
    Throughput:     8.27MB/s
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
    Reqs/sec     47963.81   17260.60  110700.27
    Latency        1.04ms     1.33ms   109.37ms
    HTTP codes:
      1xx - 0, 2xx - 93499, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6501
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6501
    Throughput:    10.25MB/s
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
    Reqs/sec    102936.74    3302.05  110007.36
    Latency      483.07us   148.23us     7.78ms
    HTTP codes:
      1xx - 0, 2xx - 95482, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4518
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4518
    Throughput:    15.56MB/s
  ```


