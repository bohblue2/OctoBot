FROM python:3.7-slim AS base

RUN apt-get update \
    && apt-get install -y --no-install-recommends build-essential gcc

RUN python -m venv /opt/venv
# Make sure we use the virtualenv:
ENV PATH="/opt/venv/bin:$PATH"

COPY . .
RUN pip install --upgrade pip \
    && pip install --prefer-binary -r requirements.txt \
    && python setup.py install

FROM python:3.7-slim

COPY --from=base /opt/venv /opt/venv

RUN octobot --create-home octobot
WORKDIR /home/octobot
USER octobot
COPY config /home/octobotconfig

# Make sure we use the virtualenv:
ENV PATH="/opt/venv/bin:$PATH"

RUN cp /opt/venv/bin/OctoBot /home/octobot/OctoBot

CMD ["./OctoBot", "-no"]
