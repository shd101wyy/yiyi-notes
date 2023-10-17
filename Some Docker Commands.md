```bash
$ docker run -it --name "ercx-container" ghcr.io/runtimeverification/ercx/ercx-runtime-env:develop
$ docker ps -a
$ docker container start ercx-container
$ docker exec -it ercx-container "/bin/bash"
$ docker logs -f --tail 100 ercx-backend-master
```