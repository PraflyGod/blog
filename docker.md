---
title: docker 
tags: rabbitmq,docker
grammar_cjkRuby: true
---

```
docker-machine create default --driver xhyve --xhyve-experimental-nfs-share --engine-registry-mirror=https://7ct4s5e2.mirror.aliyuncs.com
brew list | xargs -I % sh -c 'brew unlink %; brew link %'
eval $(docker-machine env default)
docker run -it --rm -p 8000:8000 -v $(pwd):/source lambdastack/rust-centos bash
docker exec -it 57f06afc185c /bin/bash
```

### rabbitmq
```shell
eval $(docker-machine env default)
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:management
docker exec -it ${container_id}  /bin/bash
http://192.168.64.2:15672
guest:guest
```