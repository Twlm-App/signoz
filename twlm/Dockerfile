# TWLM build of SigNoz: the official image with a rebuilt frontend.
# The backend binary is untouched — only /etc/signoz/web is replaced, with the
# UI from this branch (Integrations enabled on community self-hosted).
ARG SIGNOZ_BASE=signoz/signoz:v0.142.1

FROM node:22-bookworm-slim AS web
ENV CI=true
RUN corepack enable && corepack prepare pnpm@10 --activate
WORKDIR /src/frontend
COPY frontend/ ./
ARG VITE_VERSION=v0.142.1
RUN pnpm install --frozen-lockfile \
 && VITE_VERSION=${VITE_VERSION} NODE_OPTIONS=--max-old-space-size=8192 pnpm build

FROM ${SIGNOZ_BASE}
RUN rm -rf /etc/signoz/web
COPY --from=web /src/frontend/build/ /etc/signoz/web/
