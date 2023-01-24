# Patroni Chart [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE) [![Lifecycle:Maturing](https://img.shields.io/badge/Lifecycle-Maturing-007EC6)](https://github.com/bcgov/repomountie/blob/master/doc/lifecycle-badges.md)

A Reusable [Helm](https://helm.sh/) chart for [Patroni](https://patroni.readthedocs.io/) (Highly-Available PostgreSQL database), with a default configuration adapted to deployment on the BC Government's OpenShift cluster.

To learn more about the **Common Services** available visit the [Common Services Showcase](https://bcgov.github.io/common-service-showcase/) page.

## Directory Structure

```txt
.github/                   - PR and Issue templates
charts/                    - Helm chart root
└── patroni/               - Patroni Helm chart root
    ├── templates/         - Patroni template objects
    ├── Chart.yaml         - Patroni Chart definition
    └── values.yaml        - Patroni Chart default values
CODE-OF-CONDUCT.md         - Code of Conduct
COMPLIANCE.yaml            - BCGov PIA/STRA compliance status
CONTRIBUTING.md            - Contributing Guidelines
LICENSE                    - License
SECURITY.md                - Security Policy and Reporting
```

## Installation

### You are deploying your application server with Helm

1. Add Patroni as a dependency in your `Chart.yaml`

```yaml
dependencies:
- name: patroni
    version: 0.0.4
    repository: https://bcgov.github.io/nr-patroni-chart
    # by default, the object created will be named <your-app>-patroni. You can use an alias to override the -patroni suffix
    alias: postgres
```

2. Update your application (don't forget to update your helm dependencies with `helm dep up`)

### You are not deploying your application with Helm

If this is your first time using helm, check out the [quickstart guide](https://helm.sh/docs/intro/quickstart/). The steps below are similar to the one from the quickstart guide, but specifically for this chart

1. Add Patroni Chart repository to Helm

```sh
$ helm repo add patroni-chart https://bcgov.github.io/nr-patroni-chart
```

Once the repo is added, you can check the available charts with:

```sh
$ helm search repo patroni-chart
NAME                    CHART VERSION   APP VERSION              DESCRIPTION
patroni-chart/patroni   0.1.0           2.0.1-12.4-latest        Highly available elephant herd: HA PostgreSQL c...
```

2. Install an instance of the chart with:

```sh
$ helm install -n my-namespace my-patroni-instance patroni-chart/patroni
NAME: my-patroni-instance
LAST DEPLOYED: Mon Feb 28 15:38:32 2022
NAMESPACE: my-namespace
STATUS: deployed
REVISION: 1
```

### Configuration and default behaviour

You can find an exhaustive list of the configurable settings in [charts/patroni/values.yaml](charts/patroni/values.yaml).

#### Accessing the database cluster

This chart creates two services, allowing to access the leader (read/write connection) and replica (read-only connection) pods, respectively. The `postgres` user's password is available in the secret created by this chart under the `superuser-password` key. Note that you should only use the `postgres` user to create the user that will actually be used by your application, as [applications should not use superusers](https://patroni.readthedocs.io/en/latest/README.html#applications-should-not-use-superusers).

## Documentation

* [Security Reporting](SECURITY.md)
* [Patroni Postgres Container](https://github.com/bcgov/patroni-postgres-container)

## Getting Help or Reporting an Issue

To report bugs/issues/features requests, please file an [issue](https://github.com/BCDevOps/certbot/issues).

## How to Contribute

If you would like to contribute, please see our [contributing](CONTRIBUTING.md) guidelines.

Please note that this project is released with a [Contributor Code of Conduct](CODE-OF-CONDUCT.md). By participating in this project you agree to abide by its terms.

## License

```txt
Copyright 2022 Province of British Columbia

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
