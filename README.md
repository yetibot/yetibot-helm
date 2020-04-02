# Yetibot Helm

Yetibot Helm makes it easy to install Yetibot on a
[Kubernetes](https://kubernetes.io/) cluster.

[Yetibot's Helm chart](https://g.codefresh.io/helm/charts/CF_HELM_YETIBOT/yetibot)
is hosted on [CodeFresh](https://codefresh.io).

## Usage

```bash
# add the yetibot helm chart repo
helm repo add yetibot https://h.cfcr.io/devth/yetibot
# update to get latest charts
helm repo update
# make sure yetibot chart is listed
helm search repo yetibot
```

To install (or upgrade if already installed):

```bash
helm upgrade --ns yetibot -f values.yaml -i yetibot yetibot/yetibot
```

## Postgres

Yetibot requires a Postgres instance, and specifies the
[bitnami/postgresql](https://github.com/bitnami/charts/tree/master/bitnami/postgresql/)
chart as a dependency. It is configured in `values.yaml`, which can be overriden.

## Configuration

Though Yetibot can be configured via both environment vars or EDN, this chart
only exposes ENV-based configuration for simplicity. Env vars are settable in
`values.yaml`, which means you can set your own values override via all
available mechanisms that helm provides (typically `--set` or `-f values.yaml`).

Every key/value pair is settable under `yetibot.env`, e.g.:

```bash
helm install . \
  --namespace yetibot-test \
  --set yetibot.env.YB_URL=https://my-yeti.com \
  yetibot/yetibot
```

It's recommended to copy the default [values.yaml](values.yaml) and configure it
locally. By default this Chart configures Yetibot to connect to Freenode.net IRC
with the username `yetihelm`.

`values.yaml` demonstrates a few configuration options, but see
[sample.env](https://github.com/yetibot/yetibot.core/blob/master/config/sample.env)
for the full set of all available configuration.

## Ops

### psql export

TODO

### psql import

TODO

## Dev setup

### Helm chart on CodeFresh

Use CodeFresh Helm Museum to host the repo following [these
docs](https://codefresh.io/docs/docs/new-helm/managed-helm-repository/).

### Authentication

```bash
yarn global add codefresh
```

Follow [these
docs](https://codefresh.io/codefresh-news/introducing-codefresh-cli/) to setup
the codefresh CLI, specifically authenticating with an `API_KEY`.

```bash
read -s api_key
codefresh auth create-context --api-key $api_key
```

Now `helm push` will use the auth provided by codefresh CLI.

### Push chart

```bash
helm plugin install https://github.com/chartmuseum/helm-push
```

Push this chart up to CodeFresh (TODO move this to CI?):

```bash
helm repo add yetibot https://h.cfcr.io/devth/yetibot
helm repo list
helm lint

helm push . yetibot
```

You should see now see `yetibbot` in :

```bash
helm repo list
helm search repo
```

### Minikube

If you want to fully test out the Helm chart locally, you can use an existing
cluster or run locally on
[Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/).

After installing


### Check the rendered templates with dry run

```bash

helm install yetibot . \
  --namespace yetibot-test \
  --dry-run

# it can be useful to inspect only a subset of the output. For example, if we
# want to look at the configmap:

helm install yetibot . \
  --namespace yetibot-test \
  --dry-run | grep ConfigMap -A30

```

### Install

If everything looks good, install (this example installs it in the `yetibot`
namespace):

```bash
helm upgrade -n yetibot -i yetibot .
```

### Uninstall

If you want to delete all resources:

```bash
helm -n yetibot delete yetibot
```

*NB*: This does not delete PVCs. You can clean those up using `kubectl delete`.

### Postgres

To get the password for the postgres database, run:

```bash

export POSTGRES_PASSWORD=$(kubectl get secret --namespace default psql-postgresql -o jsonpath="{.data.postgresql-password}" | base64 --decode)
kubectl run psql-postgresql-client --rm --tty -i --restart='Never' --namespace default --image docker.io/bitnami/postgresql:11.7.0-debian-10-r51 --env="PGPASSWORD=$POSTGRES_PASSWORD" --command -- psql --host psql-postgresql -U yetibot -d yetibot -p 5432

```

### Cheat sheet

```bash

helm upgrade -n yetibot -i yetibot .

helm -n yetibot delete yetibot

helm -n yetibot list

kc delete all

kcd statefulset yetibot-postgresql-0

kc get pvc

kc delete pvc data-yetibot-postgresql-0

kc get po
kc get po -w

kc exec -it yetibot-postgresql-0 sh

# then inside the pod, verify psql:

PGPASSWORD="$POSTGRES_PASSWORD" psql -U yetibot -d yetibot

```
