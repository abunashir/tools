ARG APP_PATH="/workspace"
ARG BASE_IMAGE=gradle:8.2.1-jdk17-jammy
ARG PROD_IMAGE=eclipse-temurin:17-jre-jammy

#
# Stage: Development
#
FROM $BASE_IMAGE as development
ARG APP_PATH
WORKDIR $APP_PATH

#
# Stage: Build
#
FROM development as build
ARG APP_PATH

WORKDIR $APP_PATH
COPY . .

RUN gradle bootJar

#
# Stage: Production
#
FROM $PROD_IMAGE as production

COPY --from=build $APP_PATH/build/libs/app.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
