#

FROM registry.gitlab.factory.social.gouv.fr/socialgouv/docker/kubectl:2.9.0 AS kubectl-image
FROM registry.gitlab.factory.social.gouv.fr/socialgouv/docker/bats:2.9.0 AS bats-image

#

FROM alpine/helm:3.5.2

ARG HELM_CHART_VERSION
ENV HELM_CHART_VERSION=${HELM_CHART_VERSION}

SHELL ["/bin/ash", "-eo", "pipefail", "-c"]
RUN set -ex \
  #
  && apk add --update --no-cache \
    bash=~5 \
    bats=~1 \
    coreutils=~8 \
    gettext=~0 \
  #
  && helm repo add socialgouv "https://github.com/SocialGouv/helm-charts/releases/download/v${HELM_CHART_VERSION}" \
  ;

COPY --from=kubectl-image /usr/local/bin/kubectl /usr/local/bin/kubectl
COPY --from=kubectl-image /usr/local/bin/kustomize /usr/local/bin/kustomize
COPY --from=bats-image /usr/lib/bats-assert /usr/lib/bats-assert
COPY --from=bats-image /usr/lib/bats-support /usr/lib/bats-support
COPY --from=bats-image /usr/lib/bats-file /usr/lib/bats-file
