<!--

********************************************************************************

WARNING:

    DO NOT EDIT "varnish/README.md"

    IT IS AUTO-GENERATED

    (from the other files in "varnish/" combined with a set of templates)

********************************************************************************

-->

# Supported tags and respective `Dockerfile` links

-	[`6.2.0-stretch`, `6.2-stretch`, `6-stretch`, `stretch`, `6.2.0`, `6.2`, `6`, `latest` (*6.2/stretch/Dockerfile*)](https://github.com/coopTilleuls/docker-varnish/blob/bdb37c46dcc3c4563b698adf2cca8722432b0913/6.2/stretch/Dockerfile)
-	[`6.2.0-alpine3.8`, `6.2-alpine3.8`, `6-alpine3.8`, `alpine3.8`, `6.2.0-alpine`, `6.2-alpine`, `6-alpine`, `alpine` (*6.2/alpine3.8/Dockerfile*)](https://github.com/coopTilleuls/docker-varnish/blob/bdb37c46dcc3c4563b698adf2cca8722432b0913/6.2/alpine3.8/Dockerfile)
-	[`6.0.3-stretch`, `6.0-stretch`, `6-lts-stretch`, `lts-stretch`, `6.0.3`, `6.0`, `6-lts`, `lts` (*6.0/stretch/Dockerfile*)](https://github.com/coopTilleuls/docker-varnish/blob/bdb37c46dcc3c4563b698adf2cca8722432b0913/6.0/stretch/Dockerfile)
-	[`6.0.3-alpine3.8`, `6.0-alpine3.8`, `6-lts-alpine3.8`, `lts-alpine3.8`, `6.0.3-alpine`, `6.0-alpine`, `6-lts-alpine`, `lts-alpine` (*6.0/alpine3.8/Dockerfile*)](https://github.com/coopTilleuls/docker-varnish/blob/bdb37c46dcc3c4563b698adf2cca8722432b0913/6.0/alpine3.8/Dockerfile)
-	[`4.1.11-stretch`, `4.1-stretch`, `4-stretch`, `4-lts-stretch`, `4.1.11`, `4.1`, `4`, `4-lts` (*4.1/stretch/Dockerfile*)](https://github.com/coopTilleuls/docker-varnish/blob/bdb37c46dcc3c4563b698adf2cca8722432b0913/4.1/stretch/Dockerfile)
-	[`4.1.11-alpine3.8`, `4.1-alpine3.8`, `4-alpine3.8`, `4-lts-alpine3.8`, `4.1.11-alpine`, `4.1-alpine`, `4-alpine`, `4-lts-alpine` (*4.1/alpine3.8/Dockerfile*)](https://github.com/coopTilleuls/docker-varnish/blob/bdb37c46dcc3c4563b698adf2cca8722432b0913/4.1/alpine3.8/Dockerfile)

# Quick reference

<!-- -	**Where to get help**:  
	[the Docker Community Forums](https://forums.docker.com/), [the Docker Community Slack](https://blog.docker.com/2016/11/introducing-docker-community-directory-docker-community-slack/), or [Stack Overflow](https://stackoverflow.com/search?tab=newest&q=docker) -->

-	**Where to file issues**:  
	[https://github.com/coopTilleuls/docker-varnish/issues](https://github.com/coopTilleuls/docker-varnish/issues)

-	**Maintained by**:  
	[the Varnish Docker Community](https://github.com/coopTilleuls/docker-varnish)

<!-- -	**Supported architectures**: ([more info](https://github.com/docker-library/official-images#architectures-other-than-amd64))  
	`amd64`, `arm32v5`, `arm32v6`, `arm32v7`, `arm64v8`, `i386`, `ppc64le`, `s390x`

-	**Published image artifact details**:  
	[repo-info repo's `repos/varnish/` directory](https://github.com/docker-library/repo-info/blob/master/repos/varnish) ([history](https://github.com/docker-library/repo-info/commits/master/repos/varnish))  
	(image metadata, transfer size, etc)

-	**Image updates**:  
	[official-images PRs with label `library/varnish`](https://github.com/docker-library/official-images/pulls?q=label%3Alibrary%2Fvarnish)  
	[official-images repo's `library/varnish` file](https://github.com/docker-library/official-images/blob/master/library/varnish) ([history](https://github.com/docker-library/official-images/commits/master/library/varnish))

-	**Source of this description**:  
	[docs repo's `varnish/` directory](https://github.com/docker-library/docs/tree/master/varnish) ([history](https://github.com/docker-library/docs/commits/master/varnish)) -->

-	**Supported Docker versions**:  
	[the latest release](https://github.com/docker/docker-ce/releases/latest) (down to 1.6 on a best-effort basis)

# What is Varnish?

Varnish is an HTTP accelerator designed for content-heavy dynamic web sites as well as APIs. In contrast to other web accelerators, such as Squid, which began life as a client-side cache, or Apache and nginx, which are primarily origin servers, Varnish was designed as an HTTP accelerator. Varnish is focused exclusively on HTTP, unlike other proxy servers that often support FTP, SMTP and other network protocols.

> [wikipedia.org/wiki/Varnish_(software)](https://en.wikipedia.org/wiki/Varnish_(software))

![logo](https://raw.githubusercontent.com/docker-library/docs/5ca8e315af01e76381d499a2928f7f47a6787f49/varnish/logo.png)

# How to use this image.

## Basic usage

Create a `default.vcl` file:

```vcl
vcl 4.0;

backend default {
  .host = "www.nytimes.com";
  .port = "80";
}
```

Then run:

```console
$ docker run --name my-running-varnish -v /path/to/default.vcl:/usr/local/etc/varnish/default.vcl:ro --tmpfs /usr/local/var/varnish:exec -d cooptilleuls/varnish
```

Alternatively, a simple `Dockerfile` can be used to generate a new image that includes the necessary `default.vcl` (which is a much cleaner solution than the bind mount above):

```dockerfile
FROM cooptilleuls/varnish:6.2

COPY default.vcl /usr/local/etc/varnish/
```

Place this file in the same directory as your `default.vcl`, run `docker build -t my-varnish .`, then start your container:

```console
$ docker run --name my-running-varnish --tmpfs /usr/local/var/varnish:exec -d my-varnish
```

### Exposing the port

```console
$ docker run --name my-running-varnish --tmpfs /usr/local/var/varnish:exec -d -p 8080:80 my-varnish
```

Then you can hit `http://localhost:8080` or `http://host-ip:8080` in your browser.

# How to install VMODs (Varnish Modules)

[VMODs](https://varnish-cache.org/vmods/) are extensions written for Varnish Cache.

Install VMODs in your Varnish project's `Dockerfile`. For example, to install the [vmod-querystring](https://github.com/Dridi/libvmod-querystring) module:

```dockerfile
FROM cooptilleuls/varnish:6.2

# install vmod-querystring
ENV VMOD_QUERYSTRING_VERSION 1.0.5
RUN set -eux; \
	\
	fetchDeps=' \
		ca-certificates \
		wget \
	'; \
	buildDeps=" \
		$VMOD_BUILD_DEPS \
		dpkg-dev \
	"; \
	apt-get update; \
	apt-get install -y --no-install-recommends $fetchDeps $buildDeps; \
	rm -rf /var/lib/apt/lists/*; \
	\
	wget -O vmod-querystring.tar.gz "https://github.com/Dridi/libvmod-querystring/releases/download/v$VMOD_QUERYSTRING_VERSION/vmod-querystring-$VMOD_QUERYSTRING_VERSION.tar.gz"; \
	mkdir -p /usr/local/src/vmod-querystring; \
	tar -zxf vmod-querystring.tar.gz -C /usr/local/src/vmod-querystring --strip-components=1; \
	rm vmod-querystring.tar.gz; \
	\
	cd /usr/local/src/vmod-querystring; \
	gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
	./configure \
		--build="$gnuArch" \
	; \
	make -j "$(nproc)"; \
	make install; \
	\
	cd /; \
	rm -rf /usr/local/src/vmod-querystring; \
	\
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false $fetchDeps $buildDeps
```

# Image Variants

The `cooptilleuls/varnish` images come in many flavors, each designed for a specific use case.

## `cooptilleuls/varnish:<version>`

This is the defacto image. If you are unsure about what your needs are, you probably want to use this one. It is designed to be used both as a throw away container (mount your source code and start the container to start your app), as well as the base to build other images off of.

Some of these tags may have names like stretch in them. These are the suite code names for releases of [Debian](https://wiki.debian.org/DebianReleases) and indicate which release the image is based on.

## `cooptilleuls/varnish:<version>-alpine`

This image is based on the popular [Alpine Linux project](http://alpinelinux.org), available in [the `alpine` official image](https://hub.docker.com/_/alpine). Alpine Linux is much smaller than most distribution base images (~5MB), and thus leads to much slimmer images in general.

This variant is highly recommended when final image size being as small as possible is desired. The main caveat to note is that it does use [musl libc](http://www.musl-libc.org) instead of [glibc and friends](http://www.etalabs.net/compare_libcs.html), so certain software might run into issues depending on the depth of their libc requirements. However, most software doesn't have an issue with this, so this variant is usually a very safe choice. See [this Hacker News comment thread](https://news.ycombinator.com/item?id=10782897) for more discussion of the issues that might arise and some pro/con comparisons of using Alpine-based images.

To minimize image size, it's uncommon for additional related tools (such as `git` or `bash`) to be included in Alpine-based images. Using this image as a base, add the things you need in your own Dockerfile (see the [`alpine` image description](https://hub.docker.com/_/alpine/) for examples of how to install packages if you are unfamiliar).

# License

View [license information](https://github.com/varnishcache/varnish-cache/blob/master/LICENSE) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

<!-- Some additional license information which was able to be auto-detected might be found in [the `repo-info` repository's `varnish/` directory](https://github.com/docker-library/repo-info/tree/master/repos/varnish). -->

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.
