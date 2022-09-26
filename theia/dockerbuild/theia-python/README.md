# Python

### Build latest

```bash
docker build . -t ccr.ccs.tencentyun.com/caishengxiang/theia-python:latest
docker build . -t ccr.ccs.tencentyun.com/caishengxiang/theia-python:old
```


### Run locally on Linux or OS X

```bash
docker run -it --init -p 3000:3000 -v "$(pwd):/home/project" theiaide/theia-python:latest
docker run -it --init -p 3000:3000 -v "$(pwd):/home/project" ccr.ccs.tencentyun.com/caishengxiang/theia-python:old
```

