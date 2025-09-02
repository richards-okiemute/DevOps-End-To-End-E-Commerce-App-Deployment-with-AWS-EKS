FROM python:3.12-slim-bookworm AS build


#Set WORKDIR

WORKDIR /usr/src/app

COPY requirements.txt ./

RUN pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt


COPY . .

ENV RECOMMENDATION_PORT=9001

ENTRYPOINT ["python", "recommendation_server.py"]