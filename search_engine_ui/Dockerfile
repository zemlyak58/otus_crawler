# FROM ubuntu:18.04
FROM python:3.8

WORKDIR /home

RUN apt-get update
RUN apt-get install -y git python3-pip  #python python3-pip mongodb-server git  #rabbitmq

ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN git clone -b master https://github.com/express42/search_engine_ui

# COPY hello.py .
# COPY requirements.txt ./

RUN pip3 install --upgrade pip
RUN pip install -r /home/search_engine_ui/requirements.txt

ENV FLASK_APP=ui.py

# ENTRYPOINT ["python", "hello.py"]
ENTRYPOINT ["python", "/home/search_engine_ui/ui/ui.py"]

# RUN apt-get install -y mongodb-server ruby-full ruby-dev build-essential git
# RUN git clone -b monolith https://github.com/express42/reddit.git
# RUN gem install bundler

# COPY crawler /crawler
# COPY mongod.conf /etc/mongodb.conf
# COPY db_config /reddit/db_config
# COPY start.sh /start.sh
# COPY reddit.service /etc/systemd/system/reddit.service

# RUN cd /reddit && rm Gemfile.lock && bundle install
# RUN chmod 0777 /start.sh

# CMD ["/start.sh"]
# CMD [python -u crawler/crawler.py]
