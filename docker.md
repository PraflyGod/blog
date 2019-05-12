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