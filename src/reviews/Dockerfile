FROM cloudstudio-devops-docker.pkg.coding.net/artifacts/dev/gradle:4.8.1 AS build
USER root
COPY --chown=gralde:gradle . /home/gradle/src
WORKDIR /home/gradle/src
RUN gradle clean build --no-daemon

FROM cloudstudio-devops-docker.pkg.coding.net/artifacts/dev/websphere-liberty:22.0.0.8-full-java8-ibmjava

ENV SERVERDIRNAME reviews

COPY --from=build /home/gradle/src/reviews-wlpcfg/servers/LibertyProjectServer /opt/ol/wlp/usr/servers/defaultServer/

RUN /opt/ol/wlp/bin/featureUtility installServerFeatures  --acceptLicense /opt/ol/wlp/usr/servers/defaultServer/server.xml && \
    chmod -R g=rwx /opt/ol/wlp/output/defaultServer/

ARG service_version
ARG enable_ratings
ARG star_color
ENV SERVICE_VERSION ${service_version:-v1}
ENV ENABLE_RATINGS ${enable_ratings:-false}
ENV STAR_COLOR ${star_color:-black}

CMD ["/opt/ol/wlp/bin/server", "run", "defaultServer"]
