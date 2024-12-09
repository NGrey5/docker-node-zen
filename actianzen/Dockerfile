FROM debian:bookworm-slim as builder
COPY ./assets/zenclient.tar.gz /usr/local/zenclient.tar.gz
RUN tar -zxf /usr/local/zenclient.tar.gz --directory /usr/local
RUN rm /usr/local/zenclient.tar.gz
RUN /usr/local/actianzen/etc/clientpostinstall.sh


FROM debian:bookworm-slim
RUN apt-get update && apt-get install unixodbc -y
COPY --from=builder /usr/local/actianzen/lib64 /usr/local/actianzen/lib64
COPY --from=builder /usr/local/actianzen/etc /usr/local/actianzen/etc
RUN cat /usr/local/actianzen/etc/odbcinst.ini > /etc/odbcinst.ini

ENV LD_LIBRARY_PATH=/usr/local/actianzen/lib64