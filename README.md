# k8s-cli-jbang project

This project is an example of how to extend kubernetes cli (kubectl) with a Java application.  It demonstrates the use of Kubernetes [client-java](https://github.com/kubernetes-client/java) and access the Kubernetes API.  In order to be a kubectl plugin it is necessary to have a file which uses the defined naming convention of `kubectl-<plugin-name>`.  This is a challenge in Java, but can be done with `jbang`. In order to provide a good CLI experience [picocli](https://picocli.info/) is used.

This work and demo is provided by [Max Andersen](https://github.com/maxandersen) creator of j'bang as a followup to a [demo of the same using graal](https://github.com/codementor/k8s-cli-java).  His original work is https://github.com/maxandersen/k8s-cli-java.  What a great reminder of how the value the internet can provided!

## Prerequisites

* Running Kubernetes 1.15+ cluster.  [Kind 0.7.0](https://github.com/kubernetes-sigs/kind) was used for testing.
* Java 1.8 and [jbang 0.16+](https://github.com/maxandersen/jbang)

## j'bang

Why jbang?

The goal of this project is to reduce the experience necessary to integrate a kubectl plugin along with kubernetes interactions in a Java friendly way.   This means building an executable or script that is in the path and takes the form of "kubectl-<plugin-name>".  This can be challenging for a Java application.  One approach is to build a native executable image which is possible with graalvm.  It was also viewed that the startup time of a Java application would be too slow and native was necessary... than along comes [j'bang](https://github.com/maxandersen/jbang), providing Java shell scripting.  Counter to any performance fears, after the first compilation, it has good startup performance.

j'bang is definitely the fastest feedback loop to experiment with these features.

### jbang things to know

* [jbang installation](https://github.com/maxandersen/jbang#installation)
* Make the script `kubectl-example` in this example executable `chmod +x kubectl-example`
* The base class must match the script name where the hyphen is ignored and the 2nd word capitalized.  So `KubectlExample` is the class for `kubectl-example`.

## Getting Started

Start a cluster:  `kind create cluster`

and run one of the commands:  `./kubectl-exampl pod list`

Example:

```bash
./kubectl-example pod list
________________________________________________________________
| Pod Name                                  | namespace         |
|===============================================================|
| coredns-6955765f44-f966p                  | kube-system       |
| coredns-6955765f44-xnzbg                  | kube-system       |
| etcd-kind-control-plane                   | kube-system       |
| kindnet-cznll                             | kube-system       |
| kube-apiserver-kind-control-plane         | kube-system       |
| kube-controller-manager-kind-control-plane| kube-system       |
| kube-proxy-tw9cb                          | kube-system       |
| kube-scheduler-kind-control-plane         | kube-system       |
| local-path-provisioner-7745554f7f-gk5j9   | local-path-storage|
```

## List of Commands

* pod list
* pod list2
* pod add <pod-name> [-n namespace] [-i image]
* resources

## Adding as a kubectl plugin

The executable needs to be the path.  From the root of the project run: `export PATH=$PATH:$PWD`

Now give `kubectl` a try with `example`.  run: `kubectl example pod list`
You should get the same output as above.
