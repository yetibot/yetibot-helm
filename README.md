# Yetibot Helm

Install Yetibot on your Kubernetes cluster. Edit `values.yaml` to customize.

```bash
helm upgrade -i yetibot --namespace yetibot .
```

## Configuration

Yetibot can be configured purely from ENV or from a config file. It mostly
consists of API keys and other secrets, so it's up to the user to securely
provide them. See [Configuration
docs](https://github.com/yetibot/yetibot.core/blob/master/doc/CONFIGURATION.md)
to learn more.
