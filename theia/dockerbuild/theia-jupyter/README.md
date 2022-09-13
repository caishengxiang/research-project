# Python

### Build latest

```bash
docker build . -t theiaide/theia-jupyter:0.3.12
docker build . -t theiaide/theia-jupyter:latest
docker build . -t ccr.ccs.tencentyun.com/caishengxiang/theia-jupyter:latest
```


### Run locally on Linux or OS X

```bash
docker run -it --init -p 3000:3000 -v "$(pwd):/home/project" theiaide/theia-jupyter:latest
```

