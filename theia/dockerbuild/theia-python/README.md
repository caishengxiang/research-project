# Python

### Build latest

```bash
docker build . -t theiaide/theia-python:0.3.12
docker build . -t theiaide/theia-python:latest
```


### Run locally on Linux or OS X

```bash
docker run -it --init -p 3000:3000 -v "$(pwd):/home/project" theiaide/theia-python:latest
```

