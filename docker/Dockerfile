#
# Multi Dockerfile
#
# 実行コマンド例： docker build --target aws_builder --file docker/Dockerfile .
#
#
FROM python:3.8 as base_builder
USER root


ENV DOCKER_DIR="./docker"
ENV PRJ_DIR="/myproject"
ENV COPY_DIR="./myproject"
ENV WSGI_PY="myproject/wsgi.py"

RUN apt-get update
RUN apt-get -y install locales && \
    localedef -f UTF-8 -i ja_JP ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ JST-9
ENV TERM xterm
RUN apt-get install -y vim less

# コンテナ側へコピー
COPY $DOCKER_DIR/requirements.txt requirements.txt
# python ライブラリインストール
RUN pip install -r ./requirements.txt
# アプリケーションディレクトリ
#WORKDIR $PRJ_DIR

#-----------------------------------------------------------------------------
FROM base_builder as local_builder
EXPOSE 8000
#ENV PRJ_DIR="./myproject"
#CMD ["sh", "-c", "uwsgi --socket :8000 --chdir $PRJ_DIR --wsgi-file $WSGI_PY"]


#-----------------------------------------------------------------------------
FROM base_builder as aws_builder
# モジュールをコピー
# このディレクトリ名はコピーされないちょっとUNIXとは挙動が違う
COPY $COPY_DIR $PRJ_DIR/.

EXPOSE 8000
#CMD ["sh", "-c", "uwsgi --socket :8000 --chdir $PRJ_DIR --wsgi-file $WSGI_PY"]

