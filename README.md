# Yetibot Helm

Yetibot Helm makes it easy to install Yetibot on a
[Kubernetes](https://kubernetes.io/) cluster.

## Usage


```bash
# add the yetibot helm chart repo
helm repo add yetibot https://h.cfcr.io/devth/yetibot
# update to get latest charts
helm repo update
# make sure yetibot chart is listed
helm search repo yetibot
```

At this point the chart is ready to install. Make sure you have access to your
Kubernetes cluster and you're in the namespace that you want to install Yetibot
in.

The main configuration necessary is Yetibot's `yetibot.edn` file. Grab a copy of
the sample config and fill in any of the features you'd like to use (at the very
least configure 1 or more adapters so it has something to connect to):

```bash
curl -o yetibot.edn https://raw.githubusercontent.com/yetibot/yetibot.core/master/config/config.sample.edn
```

Install Yetibot, pointing it to the config

```bash
helm install yetibot yetibot/yetibot --version 0.1.1

```

## Configuration

Yetibot can be configured purely from ENV or from a config file. It mostly
consists of API keys and other secrets, so it's up to the user to securely
provide them. See [Configuration
docs](https://yetibot.com/ops-guide#configuration) to learn more.

As an example, this chart includes a [`config.yaml`](templates/config.yaml) with
sample config populated from `values.yaml` (which you can override) that's
mounted into the pod with a `CONFIG_PATH` env var set to the mounted location.

```bash
cp values.yaml values-overrides.yaml
helm upgrade --debug --dry-run -i -f values-overrides.yaml yetibot --namespace yetibot .
```

## Usage

```bash
helm upgrade -i yetibot --namespace yetibot .
```

### Dry run

```bash
helm upgrade --debug --dry-run -i yetibot --namespace yetibot .
```

## Postgres

Yetibot requires a Postgres instance.

## Dev setup

Use codefresh Helm Museum to host the repo following [these
docs](https://codefresh.io/docs/docs/new-helm/managed-helm-repository/).

```bash
helm plugin install https://github.com/chartmuseum/helm-push
```

```bash
helm repo add yetibot https://h.cfcr.io/devth/yetibot
helm lint
helm push . yetibot
```
