# base
ARG NODE_VERSION=16.17
FROM node:${NODE_VERSION}

# apt/yarn
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free" > /etc/apt/sources.list \
    && echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free" >> /etc/apt/sources.list


RUN apt-get update && apt-get upgrade -y

RUN apt-get install -y make build-essential libssl-dev libsecret-1-dev\
    && apt-get install -y libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
    && apt-get install -y libncurses5-dev  libncursesw5-dev xz-utils tk-dev \
    && apt-get install -y yarn \
    && apt-get clean \
    && apt-get auto-remove -y \
    && rm -rf /var/cache/apt/* \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/*

# -------------------python------------
ARG PYTHON_VERSION=3.9.7
RUN wget https://mirrors.huaweicloud.com/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz \
    && tar xvf Python-${PYTHON_VERSION}.tgz \
    && cd Python-${PYTHON_VERSION} \
    && ./configure \
    && make -j8 \
    && make install \
    && cd .. \
    && rm -rf Python-${PYTHON_VERSION} \
    && rm Python-${PYTHON_VERSION}.tgz

RUN rm /usr/bin/python && rm /usr/bin/python2 && rm /usr/bin/python2.7 \
    && ln -s /usr/local/bin/python3.9 /usr/bin/python && ln -s /usr/local/bin/pip3.9 /usr/bin/pip

# pip
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple \
   && python -m pip install --upgrade pip\
  && pip install python-language-server flake8 autopep8 pylint

# ---------------------------------------------------------------------------------


# ------------theia----------------------
RUN mkdir -p /home/theia \
    && mkdir -p /home/project
WORKDIR /home/theia
ENV HOME /home/theia
ADD package.json ./package.json
ADD plugins/ /home/theia/plugins
ARG GITHUB_TOKEN


RUN npm config set registry https://registry.npm.taobao.org
RUN yarn config set registry https://registry.npm.taobao.org -g
RUN yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass -g



RUN yarn --pure-lockfile && \
    NODE_OPTIONS="--max_old_space_size=4096" yarn theia build && \
    yarn theia download:plugins && \
    yarn --production && \
    yarn autoclean --init && \
    echo *.ts >> .yarnclean && \
    echo *.ts.map >> .yarnclean && \
    echo *.spec.* >> .yarnclean && \
    yarn autoclean --force && \
    yarn cache clean




EXPOSE 3000
ENV SHELL=/bin/bash \
    THEIA_DEFAULT_PLUGINS=local-dir:/home/theia/plugins
ENV USE_LOCAL_GIT true
ENTRYPOINT [ "node", "/home/theia/src-gen/backend/main.js", "/home/project", "--hostname=0.0.0.0" ]

