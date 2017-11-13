# Yetibot Helm

Yetibot Helm makes it easy to install Yetibot on a
[Kubernetes](https://kubernetes.io/) cluster.

## Configuration

Yetibot can be configured purely from ENV or from a config file. It mostly
consists of API keys and other secrets, so it's up to the user to securely
provide them. See [Configuration
docs](https://github.com/yetibot/yetibot.core/blob/master/doc/CONFIGURATION.md)
to learn more.

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
