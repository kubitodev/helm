[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/kubitodev&style=for-the-badge)](https://artifacthub.io/packages/search?repo=kubitodev)

# Kubito Helm Charts for Kubernetes

This repository acts like a helm chart repository for various Kubernetes applications, packaged by Kubito.

## TL;DR

```bash
helm repo add kubitodev https://charts.kubito.dev
helm search repo kubitodev
helm install example kubitodev/<chart>
```

If you had already added this repo earlier, run `helm repo update` to retrieve the latest versions of the packages. You can then run `helm search repo
kubitodev` to see the charts.

To uninstall the chart:

```bash
helm uninstall example kubitodev/<chart>
```

## License

Copyright &copy; 2022 Kubito

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
