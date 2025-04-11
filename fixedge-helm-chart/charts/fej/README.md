# FIXEdge Java on Kubernetes Helm Chart
This is the Official EPAM B2Bits Helm chart for installing and configuring FIXEdge Java on Kubernetes.

[Overview of EPAM B2Bits FIXEdge® Java](https://www.b2bits.com/trading_solutions/fixedge-java)

## Quickstart

```
$ kubectl create secret generic fixaj2-java-license-file --from-file=path/to/fixaj2-license.bin
$ kubectl create secret generic fixedge-java-license-file --from-file=path/to/fixedgej-license.bin
$ helm repo add b2bits https://morozandralek.github.io/helmcharts
$ helm install my-fixedge-java b2bits/fixedge-java
```

## Introduction

This chart bootstraps a [FIXEdge® Java](https://www.b2bits.com/trading_solutions/fixedge-java) deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.


## Prerequisites

-   Kubernetes 1.21+
-   Helm 3.9.0+
-   PV provisioner support in the underlying infrastructure
-   Valid License Key

## Obtaining a License Key

To run FIXEdge you will need a valid license key file. You can obtain a trial license from [sales@b2bits.com](mailto:sales@b2bits.com?subject=FIXEdge%20Java%20Trial%20License%20Request).

## Installing the Chart

To install the chart with the release name `my-fixedge-java`:

```
$ kubectl create secret generic fixaj2-java-license-file --from-file=path/to/fixaj2-license.bin
$ kubectl create secret generic fixedge-java-license-file --from-file=path/to/fixedgej-license.bin
$ helm repo add b2bits https://morozandralek.github.io/helmcharts
$ helm install my-fixedge-java b2bits/fixedge-java
```


These commands deploy B2Bits FIXEdge on the Kubernetes cluster in the default configuration. The **Parameters** section below lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-fixedge-java` deployment:

```
$ helm delete my-fixedge-java
```

The command removes all the Kubernetes components associated with the chart and deletes the release. 

## Parameters

| Parameter                                     | Description                                                      | Default                                            |
|:--------------------------------------------- |:---------------------------------------------------------------- |:-------------------------------------------------- |
| force_init_configs                            | Update config on re-deploy                                       | false                                              |
| git_configs.url                               | Repository with configuration for FIXEdge Java                   | https://github.com/morozandralek/b2bits-config.git |
| git_configs.branch                            | Branch from the config repository                                | main                                               |
| global.fixedge_java.image.url                 | Docker image URL for FIXEdge Java                                | b2bitsepam/fixedge-java                            |
| global.fixedge_java.image.version             | Docker image version                                             | 1.10.4                                             |
| global.fixedge_java.image.imagePullPolicy     | Image pull policy                                                | Always                                             |
| global.fixedge_java.port                      | FIXEdge Java port                                                | 8901                                               |
| global.fixedge_java.httpAdmPort               | FIXEdge Java HTTP admin port                                     | 8911                                               |
| global.fixedge_java.resources.requests.cpu    | CPU request                                                      | 0.5                                                |
| global.fixedge_java.resources.requests.memory | Memory request                                                   | 1Gi                                                |
| global.fixedge_java.resources.limits.cpu      | CPU limit                                                        | 0.5                                                |
| global.fixedge_java.resources.limits.memory   | Memory limit                                                     | 1Gi                                                |
| global.storage.accessModes                    | Storage access mode                                              | ReadWriteOnce                                      |
| global.storage.fe_java_session_logs.size      | Session logs storage size                                        | 10Gi                                               |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install my-fixedge-java --set force_init_configs=true,fixedge.resources.limits.memory=800Mi b2bits/fixedge-java
```

The above command forces pull configuration on start (`force_init_configs=true`) and sets RAM limit to `800Mi`.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install my-fixedge-java -f values.yaml b2bits/fixedge-java
```

> **Tip**: You can use the default [values.yaml](values.yaml) for reference.



## Configuration and installation details

### Using custom configuration

By default this helm chart clones FIXEdge configuration from `https://morozandralek.github.io/fixedge`.

If you choose to maintain your custom FIXEdge configuration in private Git repository, you shuold provide SSH details for access to remote repository:
```
$ kubectl create secret generic ssh-creds --from-file=path/to/known_hosts --from-file=path/to/id_rsa
```

```
$ helm install my-fixedge-java --set git_configs.url=<your-git-repo-ssh-url>,git_configs.branch=<your-git-repo-branch> b2bits/fixedge-java
```

It is expected that all FIXEdge configuration files are grouped under `fixedge-java` folder in your repository.

## Persistence
The [EPAM B2Bits FIXEdge Java](https://hub.docker.com/r/b2bitsepam/fixedge-java) image stores the FIXEdge data at the `/var/lib/fixedge-java` path of the container.

Persistent Volume Claims are used to keep the data across deployments. This is known to work in AWS, Azure, minikube and k3s.
See the [Parameters](#parameters) section to configure the PVC.

## Where to get help

### Sales Information

Email: [sales@b2bits.com](mailto:sales@b2bits.com?subject=FIXEdge%20Java%20Details%20Request)

### Technical Queries

For technical queries please contact [SupportFIXAntenna@epam.com](mailto:SupportFIXAntenna@epam.com)

## License

Copyright © B2BITS EPAM Systems Company 2000-2025 

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
