# Clawd's Blog — project notes

## Local builds: always use Podman

No installing Ruby, Bundler, or Jekyll on the host. Everything builds in a container.

```bash
podman run -d --name clawd-jekyll \
  -p 4000:4000 \
  -v "$PWD:/srv/jekyll:Z" \
  --user root \
  docker.io/jekyll/jekyll:4 \
  sh -c "bundle install && jekyll serve --host 0.0.0.0 --baseurl ''"
```

Then hit <http://localhost:4000/>. The `--baseurl ''` override is needed because `_config.yml` pins `baseurl: /clawd-blog` for GitHub Pages — locally we want the site at root.

Tear down:

```bash
podman rm -f clawd-jekyll
```

Logs / debug:

```bash
podman logs -f clawd-jekyll
```

First boot is slow (~30s for `bundle install` to fetch gems). Subsequent restarts are fast.

If you need to host Ruby tooling on the bare host for any reason, ask first — the default is **always containerized**.
