# Use Alpine
# https://hub.docker.com/_/alpine/tags?name=3.22
ARG ALPINE_VERSION=3.22.1
ARG ALPINE_IMAGE_NAME=alpine
ARG A_PARTICULAR_TAG_NAME="a-particular-tag"
ARG A_PARTICULAR_TAG_SEMVER=5.4.3

# Builder image's registry
ARG BUILDER_IMAGE_REGISTRY=docker.io
ARG BUILDER_IMAGE_NAME=${ALPINE_IMAGE_NAME}
ARG BUILDER_IMAGE_TAG=${ALPINE_VERSION}

# For OCI labels
ARG BASE_REGISTRY=${BUILDER_IMAGE_REGISTRY}
ARG BASE_IMAGE=${BUILDER_IMAGE_NAME}
ARG BASE_IMAGE_TAG=${BUILDER_IMAGE_TAG}

# non-root user
ARG APP_USER="iamnotroot"
ARG APP_USER_UID="1234"
ARG APP_GROUP="iamnotroot"
ARG APP_GROUP_GID="5678"

#==============================================================================#
# Alpine base image & create non root user
#==============================================================================#
FROM ${BUILDER_IMAGE_REGISTRY}/${BUILDER_IMAGE_NAME}:${BUILDER_IMAGE_TAG} AS builder-base

# are defined at the begining og the file
ARG APP_USER
ARG APP_GROUP
ARG APP_USER_UID
ARG APP_GROUP_GID

# create a non-root user since this app does not need root privileges
RUN addgroup \
        -g ${APP_GROUP_GID} \
        ${APP_GROUP}
    
RUN adduser \
        -u ${APP_USER_UID} \
        -G ${APP_GROUP} \
        -s "/bin/sh" \
        -D \
        -g "non root user" \
        ${APP_USER}

RUN touch /home/${APP_USER}/first-stage-container-build.txt && \
    echo "this is the first build stage" > /home/${APP_USER}/second-stage-container-build.txt

#==============================================================================#
# Final image without labels/annotations
#==============================================================================#
FROM builder-base as final

RUN touch /home/${APP_USER}/final-stage-container-build.txt && \
    echo "this is the final build stage" > /home/${APP_USER}/final-stage-container-build.txt


ENTRYPOINT ["/bin/bash"]

#==============================================================================#
# Final image with labels/annotations
#==============================================================================#
FROM final as final-annotations

# See https://github.com/opencontainers/image-spec/blob/main/annotations.md
ARG LABEL_CREATED=""
ARG LABEL_AUTHOR="My Email <my.email@adomain.com>"
ARG LABEL_URL="ghcr.io/nicolas-peiffer/testing-github-actions"
ARG LABEL_DOCUMENTATION="https://github.com/Nicolas-Peiffer/testing-github-actions"
ARG LABEL_SOURCE="https://github.com/Nicolas-Peiffer/testing-github-actions"
ARG LABEL_VERSION=""
ARG LABEL_REVISION=""
ARG LABEL_VENDOR="My Vendor"
ARG LABEL_LICENSES="MIT"
ARG LABEL_TITLE="My Container Image"
ARG LABEL_REF_NAME=""
ARG LABEL_DESCRIPTION="The purpose of this project is to test github action that build containers"
ARG LABEL_BASE_DIGEST=""
ARG BASE_REGISTRY
ARG BASE_IMAGE
ARG BASE_IMAGE_TAG
ARG LABEL_BASE_NAME="${BASE_REGISTRY}/${BASE_IMAGE}:${BASE_IMAGE_TAG}"
LABEL org.opencontainers.image.created="${LABEL_CREATED}"
LABEL org.opencontainers.image.authors="${LABEL_AUTHOR}"
LABEL org.opencontainers.image.url="${LABEL_URL}"
LABEL org.opencontainers.image.documentation="${LABEL_DOCUMENTATION}"
LABEL org.opencontainers.image.source="${LABEL_SOURCE}"
LABEL org.opencontainers.image.version="${LABEL_VERSION}"
LABEL org.opencontainers.image.revision="${LABEL_REVISION}"
LABEL org.opencontainers.image.vendor="${LABEL_VENDOR}"
LABEL org.opencontainers.image.licenses="${LABEL_LICENSES}"
LABEL org.opencontainers.image.title="${LABEL_TITLE}"
LABEL org.opencontainers.image.ref.name="${LABEL_REF_NAME}"
LABEL org.opencontainers.image.description="${LABEL_DESCRIPTION}"
LABEL org.opencontainers.image.base.digest="${LABEL_BASE_DIGEST}"
LABEL org.opencontainers.image.base.name="${LABEL_BASE_NAME}"
