# base image
FROM eclipse-temurin:21-jre-jammy

# maintainer
MAINTAINER xuxueli

# set timezone
ENV TZ=PRC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# create directory for SQL file
RUN mkdir -p /etc/xxl-job

# copy SQL initialization script
COPY doc/db/tables_xxl_job.sql /etc/xxl-job/tables.sql

# copy jar
ADD xxl-job-admin/target/xxl-job-admin-*.jar /app.jar

# Spring Boot SQL initialization configuration
# By default, SQL initialization is enabled. To disable automatic database initialization,
# set SPRING_SQL_INIT_MODE=never (e.g., in docker-compose or via -e flag).
ENV SPRING_SQL_INIT_MODE=always \
    SPRING_SQL_INIT_SCHEMA_LOCATIONS=file:/etc/xxl-job/tables.sql \
    SPRING_SQL_INIT_CONTINUE_ON_ERROR=true

# command
# Spring Boot properties can be passed as environment variables:
# - SPRING_DATASOURCE_URL=jdbc:mysql://mysql:3306/xxl_job?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai
# - SPRING_DATASOURCE_USERNAME=root
# - SPRING_DATASOURCE_PASSWORD=root_pwd
# - SPRING_SQL_INIT_MODE=never (to disable automatic database initialization, default is 'always')
# log home: -e LOG_HOME=/data/applogs
# jvm options: -e JAVA_OPTS="-Xms128m -Xmx128m"
CMD ["sh", "-c", "java ${LOG_HOME:+-DLOG_HOME=$LOG_HOME} ${JAVA_OPTS} -jar /app.jar"]