# DO NOT EDIT! Generated Dockerfile for cmd/auth_proxy.
ARG GO_BUILDER=registry.ci.openshift.org/openshift/release:rhel-9-release-golang-1.26-openshift-4.23
ARG GO_RUNTIME=registry.access.redhat.com/ubi9/ubi-minimal

FROM $GO_BUILDER as builder

WORKDIR /workspace
COPY . .

ENV CGO_ENABLED=1
ENV GOEXPERIMENT=strictfipsruntime

RUN go build -tags strictfipsruntime -o /usr/bin/main ./cmd/auth_proxy

FROM $GO_RUNTIME

ARG VERSION=knative-v1.21
RUN microdnf update tzdata -y && microdnf reinstall tzdata -y

COPY --from=builder /usr/bin/main /usr/bin/auth_proxy
COPY LICENSE /licenses/

USER 65532

LABEL \
      com.redhat.component="openshift-serverless-1-eventing-auth-proxy-rhel9-container" \
      name="openshift-serverless-1/eventing-auth-proxy-rhel9" \
      version=$VERSION \
      summary="Red Hat OpenShift Serverless 1 Eventing Auth Proxy" \
      maintainer="serverless-support@redhat.com" \
      description="Red Hat OpenShift Serverless 1 Eventing Auth Proxy" \
      io.k8s.display-name="Red Hat OpenShift Serverless 1 Eventing Auth Proxy" \
      io.k8s.description="Red Hat OpenShift Serverless Eventing Auth Proxy" \
      io.openshift.tags="auth-proxy" \
      vendor="Red Hat, Inc." \
      release=$VERSION \
      cpe="cpe:/a:redhat:openshift_serverless:1.38::el9"

ENTRYPOINT ["/usr/bin/auth_proxy"]
