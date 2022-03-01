# Stoplight Prism Helm Chart

This Helm chart provides an easy-2-go API mock or proxy using [Stoplight Prism](https://github.com/stoplightio/prism).

## Supported use case

Currently, the Helm chart supports:

- Publicly hosted spec on Git
- Privately hosted spec on Github, using a Github App
    - Github App provided through [ExternalSecret](https://github.com/external-secrets/kubernetes-external-secrets)

# List of improvement

- [ ] Support fetch from S3
- [ ] Support fetch from HTTP(s)
- [ ] Improve the `service.origin` so it contains an absolute path
- [ ] Harden the init container

## Usage

[Helm](https://helm.sh) must be installed and initialized to use the charts.
Please refer to Helm's [documentation](https://helm.sh/docs/) to get started.

Once Helm is set up properly, add the repo as follows. Make sure to replace `<Version>` with one of the version available down bellow:

```console
$ helm repo add stint-opensource https://raw.githubusercontent.com/stintco/stoplight-prism-helm/<Version>/
```

### Version

| Version                       | Description                                                                                                                 | Suggestion                              |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| `v1.0.0-RC1` (_Stable_)        | This is the most stable release, however, it won't come with any support!                                                   | Recommended for any live product
| `main` (_Unstable_)           | This is the latest version of the library. It is likely to contain breaking changes as it grows!                            | Recommended for early development or PoC which are likely to take a certain time before being consider stable. **Make sure to use the stable tag once your project becomes stable!**
| `v1.0.0` (**Unavailable yet**) | This version is scheduled to be released when the chart will be used widely and we have gained confidence on its stability. | -
| \<branchName>                 | You can use any branch name.                                                                                                | When working on a fix or a new feature, this is the easier way to test your library within the project which is depending of this change.

## Installing the Chart

To install the chart with the release name `my-service-mock`:

```console
$ helm install my-service-mock stoplight-prism
```

Here is a complete example:

```console
$ helm repo add stint-opensource https://raw.githubusercontent.com/stintco/stoplight-prism-helm/main/
$ helm repo update
# -- Using a public repo
$ helm install my-service-mock stint-opensource/stoplight-prism --namespace my-namespace \
    --create-namespace --wait --timeout 5m \
    --set imagePullSecrets[0].name=docker-config-default \
    --set service.name=my-service \
    --set service.origin=github.com/stintco/stoplight-prism-helm.git \
    --set service.specPath=examples/openapi.yaml \
    --set service.branch=main \
    --set service.probe=/pets \
    --set ingress.host=service.internal
# -- Using a private repo and a GH app with access to it
$ helm install my-service-mock stint-opensource/stoplight-prism --namespace my-namespace \
    --create-namespace --wait --timeout 5m \
    --set imagePullSecrets[0].name=docker-config-default \
    --set service.name=my-service \
    --set service.origin=github.com/stintco/stoplight-prism-helm.git \
    --set service.specPath=examples/openapi.yaml \
    --set service.branch=main \
    --set service.probe=/pets \
    --set github.appId=123456 \
    --set github.installId=12345678 \
    --set github.secret.type=external-secret \
    --set github.secret.backend=systemManager \
    --set github.secret.name=/my/ssm/with/app_private_key \
    --set ingress.host=service.internal
```

## Uninstalling the Chart

To uninstall the chart with the release name `my-service-mock`:

```console
$ helm uninstall my-service-mock
```

## Configuration

| Parameter                     | Description                                                                               | Default           |
|-------------------------------|-------------------------------------------------------------------------------------------|-------------------|
| `image.repository`            | The Stoplight Prism docker repo to use                                                    | `stoplight/prism` |
| `image.tag`                   | The Docker image tag                                                                      | `4`               |
| `image.pullPolicy`            | The k8s pull policy                                                                       | `IfNotPresent`    |
| `imagePullSecrets`            | The Docker secrets to use. Must be an array of object with the attribute `name` set.      | `[]`              |
| `deployment.annotations`      | The deployment annotations                                                                | `{}`              |
| `resources.limits.cpu`        | The Prism container CPU limit                                                             | `"150m"`          |
| `resources.limits.memory`     | The Prism container RAM limit                                                             | `"250Mi"`         |
| `resources.requests.cpu`      | The Prism container CPU request                                                           | `"50m"`           |
| `resources.requests.memory`   | The Prism container RAM request                                                           | `"100Mi"`         |
| `github.appId`                | The Github App ID. Note: If provided, make sure to comple the Github values. _(Optional)_ |
| `github.installId`            | The Github App installation ID. Can be found using the [Github documentation](https://docs.github.com/en/developers/apps/building-github-apps/authenticating-with-github-apps) _(Optional)_                                                                |
| `github.secret.type`          | The type of secret. This correspond to the `kind` in K8S API. Only `external-secret` is supported at the moment.                                                                  |
| `github.secret.backend`       | The type of backend when using `external-secret`. Refer to the documentation for further details. |  |
| `github.secret.name`          | The secret name. Vary depending of the use backend                                        |                   |
| `github.secret.role`          | The role to use when fetching the external secret. _(Optional)_                           |                   |
| `github.secret.region`        | The region in which to fetch the external secret. _(Optional)_                            |                   |
| `service.mode`                | The Prism mode to use, either `mock` or `proxy`                                           | `mock`            |
| `service.upstream`            | If using `proxy`, the upstream API endpoint, ignored otherwise.                           |                   |
| `service.name`                | The service nickname                                                                      | `service`         |
| `service.origin`              | The Git remote address. **Only https is supported, but you need not to include `https://`**. This is because the access token will be injected before the domain.                                                                   |                 |
| `service.specPath`            | The path to the OA3 spec                                                                  | `openapi.yaml`    |
| `service.branch`              | The branch where to find it                                                               | `master`          |
| `service.probe`               | The path can be `GET` on the API to probe readiness and liveness. Must return a status code < 400. DIsabled if not provided. _(Optional)_                                                                    |
| `ingress.enabled`             | Whether or not to create an ingress. Will only create a service otherwise, which can be used for in-cluster mocking. | `false` |
| `ingress.host`                | The ingress `host`. The TLS certificate must be available without explicit secret. You can set a wildcard to achieve this. | `service.internal` |
| `ingress.ingressClassName`    | The ingress class name to use                                                             | `""`              |
| `ingress.annotations`         | The ingress annotations                                                                   | `{}`              |
| `labels`                      | Labels to put on each resources created by this chart                                     | `{}`              |
