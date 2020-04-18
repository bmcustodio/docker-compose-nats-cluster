# docker-compose-nats-cluster

A three-node [NATS](https://nats.io/) cluster running on top of [Docker Compose](https://docs.docker.com/compose/), for testing purposes.

![License](https://img.shields.io/github/license/bmcustodio/docker-compose-nats-cluster)

## Starting

To start the NATS cluster, run

```
$ docker-compose up
(...)
nats-2_1  | [1] 2020/03/07 13:40:29.890917 [INF] Starting nats-server version 2.1.4
(...)
nats-1_1  | [1] 2020/03/07 13:40:29.841022 [INF] Starting nats-server version 2.1.4
(...)
nats-3_1  | [1] 2020/03/07 13:40:29.895016 [INF] Starting nats-server version 2.1.4
(...)
```

## Connecting

To connect to the NATS cluster, point your NATS client at one of `127.0.0.1:{14222,24222,34222}`.

### Example

Install `nats-pub` and `nats-sub`:

```shell
$ go get github.com/nats-io/nats.go/examples/nats-pub
$ go get github.com/nats-io/nats.go/examples/nats-sub
```

Point `nats-sub` at `nats://127.0.0.1:14222` and at the `foo` subject:

```
$ nats-sub -s nats://127.0.0.1:14222 foo
Listening on [foo]
```

Now, in another shell, point `nats-pub` at `nats://127.0.0.1:24222` and `nats://127.0.0.1:34222` and at the `foo` subject:

```
$ nats-pub -s nats://127.0.0.1:24222 foo bar
Published [foo] : 'bar'
$ nats-pub -s nats://127.0.0.1:34222 foo baz
Published [foo] : 'baz'
```

By then, you should start seeing messages arriving in the shell where `nats-sub` is running:

```
$ nats-sub -s nats://127.0.0.1:14222 foo
Listening on [foo]
[#1] Received on [foo] : 'bar'
[#2] Received on [foo] : 'baz'
```

## Stopping

To stop the NATS cluster, hit `Ctrl+C`.

## Destroying

To destroy the NATS cluster, run

```
$ docker-compose down
Removing docker-compose-nats-cluster_nats-1_1 ... done
Removing docker-compose-nats-cluster_nats-3_1 ... done
Removing docker-compose-nats-cluster_nats-2_1 ... done
Removing network docker-compose-nats-cluster_main
```

## License

Copyright 2017-2020 bmcustodio

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
