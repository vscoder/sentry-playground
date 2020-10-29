# Install sentry in isolated environment

Based on [this](https://medium.com/@andrenit/docker-and-artifactory-as-a-registry-mirror-1365891b39c5) article

## Environment

Here is 2 instances:
- `sentry-server` with no access to the internet and already downloaded [sentry self-hosted repository](https://github.com/getsentry/onpremise)
- `registry` with access to the internet and hosting docker registry as docker hub mirror, like described [here](https://docs.docker.com/registry/recipes/mirror/)

## TODO

- [ ] Automate up of docker registry (fix `provision.yml`)
- [ ] Configure docker on `sentry-server` to use `registry` as mirror
- [ ] Download sentry/onpremise on `sentry-server`
- [ ] Disable internet on `sentry-server` (delete default gateway)
- [ ] Try to install sentry w/o instarnet access
