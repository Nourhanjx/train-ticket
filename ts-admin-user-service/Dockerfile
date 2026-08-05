FROM mirror.gcr.io/library/eclipse-temurin:8-jre

RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone

ADD ./target/ts-admin-user-service-1.0.jar /app/
CMD ["sh", "-c", "java $JAVA_OPTS -jar /app/ts-admin-user-service-1.0.jar"]

EXPOSE 16115