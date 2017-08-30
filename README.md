# docker-compose-nats-cluster

A three-node [NATS](https://nats.io/) cluster running on top of [Docker Compose](https://docs.docker.com/compose/) for testing purposes.

## Starting

To start the cluster run

```
$ docker-compose up
(...)
nats-1_1  | [1] 2017/08/04 14:21:12.490521 [INF] Starting nats-server version 1.0.2
nats-3_1  | [1] 2017/08/04 14:21:12.544860 [INF] Starting nats-server version 1.0.2
nats-2_1  | [1] 2017/08/04 14:21:12.584499 [INF] Starting nats-server version 1.0.2
```

## Connecting

To connect to the cluster point your NATS client at one of `127.0.0.1:{14222,24222,34222}`:

```
$ nats-sub -s nats://127.0.0.1:14222 foo
Listening on [foo]
```

Now, on another shell run:

```
$ nats-pub -s nats://127.0.0.1:24222 foo bar
Published [foo] : 'bar'
$ nats-pub -s nats://127.0.0.1:34222 foo bar
Published [foo] : 'baz'
```

If everything's OK you should start seeing messages arriving in the first shell:

```
$ nats-sub -s nats://127.0.0.1:14222 foo
Listening on [foo]
[#1] Received on [foo] : 'bar'
[#2] Received on [foo] : 'baz'
```

**NOTE:** In case you're wondering, `nats-pub` and `nats-sub` come from [`ruby-nats`](https://github.com/nats-io/ruby-nats).

## Stopping

To stop the cluster hit `Ctrl+C` or run

```
$ docker-compose down
Stopping dockercomposenatscluster_nats-2_1 ... done
Stopping dockercomposenatscluster_nats-1_1 ... done
Stopping dockercomposenatscluster_nats-3_1 ... done
(...)
```

## License

Copyright 2017 brunomcustodio

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
