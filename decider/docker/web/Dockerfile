FROM python:3.8-bullseye

# install dir
WORKDIR /opt/decider

# make non-root app user
RUN adduser --no-create-home --system --shell /bin/false decider && \
    usermod --lock decider && \
    groupadd decider && \
    usermod -aG decider decider

# for CRLF -> LF
RUN apt-get update && \
    apt-get install dos2unix

# pip reqs
COPY ["./decider/requirements-pre.txt", "./decider/requirements.txt", "./"]
RUN pip install --no-cache-dir -r requirements-pre.txt && \
    pip install --no-cache-dir -r requirements.txt

# app files
COPY ["./decider/app", "./app"]
COPY ["./decider/decider.py", "./decider/docker/web/root_files/*", "./"]

# perform CRLF -> LF
RUN dos2unix entrypoint.sh

# own app files | swap user | run
RUN chown -R decider:decider .
USER decider:decider
ENTRYPOINT ["/bin/sh", "entrypoint.sh"]
