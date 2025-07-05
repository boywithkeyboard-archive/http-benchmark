## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72742` | `4000` | `83012` |
| **85%** | [Hyper Express](#hyper-express) | `61922` | `3352` | `64520` |
| **38%** | [Node (Default)](#node-default) | `27505` | `8243` | `57323` |
| **33%** | [Fastify](#fastify) | `23929` | `6917` | `35811` |
| **28%** | [Hono](#hono) | `20437` | `5440` | `30817` |
| **27%** | [Koa](#koa) | `19412` | `8203` | `61340` |
| **11%** | [Carbon](#carbon) | `8296` | `1403` | `10367` |
| **9%** | [Express](#express) | `6553` | `1110` | `8472` |


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
    Reqs/sec      8999.64    5046.92   58655.84
    Latency        5.54ms     4.30ms   371.46ms
    HTTP codes:
      1xx - 0, 2xx - 92234, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7766
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7766
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
    Reqs/sec      6993.74    5192.00   58572.08
    Latency        7.14ms     3.95ms   361.01ms
    HTTP codes:
      1xx - 0, 2xx - 90158, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9842
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9842
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
    Reqs/sec     24788.82    7500.26   35997.25
    Latency        2.02ms     2.08ms   186.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     20332.79    5360.47   30250.00
    Latency        2.46ms     1.91ms   172.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     60546.59    3247.07   63392.34
    Latency      823.84us    70.96us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.60MB/s
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
    Reqs/sec     19173.79    6654.70   50609.83
    Latency        2.60ms     2.32ms   205.75ms
    HTTP codes:
      1xx - 0, 2xx - 94381, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5619
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5619
    Throughput:     4.09MB/s
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
    Reqs/sec     26328.95    8723.30   62817.40
    Latency        1.89ms     1.97ms   164.02ms
    HTTP codes:
      1xx - 0, 2xx - 96532, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3468
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3468
    Throughput:     5.82MB/s
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
    Reqs/sec     72610.83    4057.70   80260.89
    Latency      686.66us   163.77us     6.48ms
    HTTP codes:
      1xx - 0, 2xx - 97547, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2453
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2453
    Throughput:    11.20MB/s
  ```


