<p align="center">
  <a href="https://github.com/tarampampam/3proxy-docker#readme">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://socialify.git.ci/tarampampam/3proxy-docker/image?description=1&font=Raleway&forks=1&issues=1&logo=https%3A%2F%2Fgithub.com%2Fuser-attachments%2Fassets%2F023186cf-b153-459c-8417-038fd87a2065&owner=1&pulls=1&pattern=Solid&stargazers=1&theme=Dark">
      <img align="center" src="https://socialify.git.ci/tarampampam/3proxy-docker/image?description=1&font=Raleway&forks=1&issues=1&logo=https%3A%2F%2Fgithub.com%2Fuser-attachments%2Fassets%2F023186cf-b153-459c-8417-038fd87a2065&owner=1&pulls=1&pattern=Solid&stargazers=1&theme=Light">
    </picture>
  </a>
</p>

<p align="center">
  <a href="https://github.com/tarampampam/3proxy-docker/actions"><img src="https://img.shields.io/github/actions/workflow/status/tarampampam/3proxy-docker/tests.yml?branch=master&maxAge=30&label=tests&logo=github&style=flat-square" alt="" /></a>
  <a href="https://github.com/tarampampam/3proxy-docker/actions"><img src="https://img.shields.io/github/actions/workflow/status/tarampampam/3proxy-docker/release.yml?maxAge=30&label=release&logo=github&style=flat-square" alt="" /></a>
  <a href="https://hub.docker.com/r/tarampampam/3proxy"><img src="https://img.shields.io/docker/pulls/tarampampam/3proxy.svg?maxAge=30&label=pulls&logo=docker&logoColor=white&style=flat-square" alt="" /></a>
  <a href="https://hub.docker.com/r/tarampampam/3proxy"><img src="https://img.shields.io/docker/image-size/tarampampam/3proxy/latest?maxAge=30&label=size&logo=docker&logoColor=white&style=flat-square" alt="" /></a>
  <a href="https://github.com/tarampampam/3proxy-docker/blob/master/LICENSE"><img src="https://img.shields.io/github/license/tarampampam/3proxy-docker.svg?maxAge=30&style=flat-square" alt="" /></a>
</p>

# Docker image with [3proxy][link_3proxy]

3proxy is a powerful and lightweight proxy server. This image includes the stable version and can be easily
configured using environment variables. By default, it operates with anonymous proxy settings to hide client
information and logs activity in JSON format.

> Page on `hub.docker.com` can be [found here][link_docker_hub].

TCP ports:

| Port number | Description                                             |
|-------------|---------------------------------------------------------|
| `3128`      | [HTTP proxy](https://3proxy.org/doc/man8/proxy.8.html)  |
| `1080`      | [SOCKS proxy](https://3proxy.org/doc/man8/socks.8.html) |

## Quick Start (docker-compose / Dokploy)

This repo ships a root [`docker-compose.yml`](docker-compose.yml) and an [`.env.example`](.env.example) for one-command deploys.

```bash
git clone https://github.com/Romanov-Art/3proxy-docker.git
cd 3proxy-docker
cp .env.example .env      # then edit .env — CHANGE PROXY_LOGIN / PROXY_PASSWORD
docker compose up -d
```

**Deploy on [Dokploy](https://dokploy.com/):**

1. Create a new **Compose** service and point it at this repository (branch `master`).
2. Set **Compose Path** to `docker-compose.yml`.
3. Paste the contents of `.env.example` into the service **Environment** tab and change the credentials.
4. Click **Deploy**. Ports `PROXY_PORT` (HTTP) and `SOCKS_PORT` (SOCKS5) are published on the host.

By default the prebuilt image `ghcr.io/tarampampam/3proxy:2` is used. To enable
`DNS_CACHE_SIZE`, `PROXY_EXTRA_ARGS` and `SOCKS_EXTRA_ARGS`, uncomment the
`build:` block in `docker-compose.yml` so the image is built from this repo.

See [all supported variables](#supported-environment-variables) below.

## Supported tags

| Registry                               | Image                        |
|----------------------------------------|------------------------------|
| [GitHub Container Registry][link_ghcr] | `ghcr.io/tarampampam/3proxy` |
| [Docker Hub][link_docker_hub] (mirror) | `tarampampam/3proxy`         |

> [!NOTE]
> It’s recommended to avoid using the `latest` tag, as **major** upgrades may include breaking changes.
> Instead, use specific tags in `X.Y.Z` format for version consistency.

All supported image tags can be [found here][link_docker_tags].

> Multi-arch images are published: `amd64`, `arm64`, `arm/v6`, `arm/v7`, `ppc64le`, `s390x`.

## Supported Environment Variables

| Variable Name        | Description                                                                                                           | Example                           |
|----------------------|-----------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| `PROXY_LOGIN`        | Authorization login (empty by default)                                                                                | `username`                        |
| `PROXY_PASSWORD`     | Authorization password (empty by default)                                                                             | `password`                        |
| `EXTRA_ACCOUNTS`     | Additional proxy users (JSON object format)                                                                           | `{"evil":"live", "guest":"pass"}` |
| `PRIMARY_RESOLVER`   | Primary DNS resolver (`1.0.0.1` by default)                                                                           | `8.8.8.8:5353/tcp`                |
| `SECONDARY_RESOLVER` | Secondary DNS resolver (`8.8.4.4` by default)                                                                         | `2001:4860:4860::8844`            |
| `MAX_CONNECTIONS`    | Maximum number of connections (`1024` by default)                                                                     | `2056`                            |
| `PROXY_PORT`         | HTTP proxy port (`3128` by default)                                                                                   | `8080`                            |
| `SOCKS_PORT`         | SOCKS proxy port (`1080` by default)                                                                                  | `8888`                            |
| `EXTRA_CONFIG`       | Additional 3proxy configuration (appended to the **end** of the config file, but before `proxy` and `flush`)          | `# line 1\n# line 2`              |
| `LOG_OUTPUT`         | Path for log output (`/dev/stdout` by default; set to `/dev/null` to disable logging)                                 | `/tmp/3proxy.log`                 |
| `DNS_CACHE_SIZE`     | DNS cache size (`65536` by default). **Requires building from this repo** — not applied by the prebuilt image.        | `131072`                          |
| `PROXY_EXTRA_ARGS`   | Extra args appended to the `proxy` line. **Requires building from this repo** — not applied by the prebuilt image.     | `-n`                              |
| `SOCKS_EXTRA_ARGS`   | Extra args appended to the `socks` line. **Requires building from this repo** — not applied by the prebuilt image.     | `-n`                              |

## Helm Chart

To install it on Kubernetes (K8s), please use the Helm chart from [ArtifactHUB][artifact-hub].

[artifact-hub]:https://artifacthub.io/packages/helm/proxy-3proxy/proxy-3proxy

## Plain `docker run`

Prefer [docker-compose](#quick-start-docker-compose--dokploy) above. For a quick one-off:

```bash
docker run --rm -d \
  -p "3128:3128/tcp" \
  -p "1080:1080/tcp" \
  -e "PROXY_LOGIN=User" \
  -e "PROXY_PASSWORD=Password" \
  -e "PRIMARY_RESOLVER=1.1.1.1" \
  ghcr.io/tarampampam/3proxy:2
```

## Support

[![Issues][badge_issues]][link_issues]
[![Issues][badge_pulls]][link_pulls]

If you encounter any issues, please [open an issue][link_create_issue] in this repository.

## License

This project is licensed under the WTFPL. Use it freely and enjoy!

[badge_issues]:https://img.shields.io/github/issues/tarampampam/3proxy-docker.svg?style=flat-square&maxAge=180
[badge_pulls]:https://img.shields.io/github/issues-pr/tarampampam/3proxy-docker.svg?style=flat-square&maxAge=180
[link_issues]:https://github.com/tarampampam/3proxy-docker/issues
[link_pulls]:https://github.com/tarampampam/3proxy-docker/pulls
[link_create_issue]:https://github.com/tarampampam/3proxy-docker/issues/new
[link_docker_tags]:https://hub.docker.com/r/tarampampam/3proxy/tags
[link_docker_hub]:https://hub.docker.com/r/tarampampam/3proxy/
[link_ghcr]:https://github.com/tarampampam/3proxy-docker/pkgs/container/3proxy
[link_3proxy]:https://github.com/3proxy/3proxy
