# Yetibot Helm

Install Yetibot on your Kubernetes cluster. Edit [`values.yaml`](values.yaml)
and `templates/config.yaml` to customize.

```bash
helm upgrade -i yetibot --namespace yetibot .
```

## Configuration

Yetibot can be configured purely from ENV or from a config file. It mostly
consists of API keys and other secrets, so it's up to the user to securely
provide them. See [Configuration
docs](https://github.com/yetibot/yetibot.core/blob/master/doc/CONFIGURATION.md)
to learn more.

As an example, this chart includes a [`config.yaml`](templates/config.yaml) with
sample config that's mounted into the pod with a `CONFIG_PATH` env var set to
the mounted location.
