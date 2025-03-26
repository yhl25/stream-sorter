# Stream Sorter
Numaflow pipeline that sorts incoming messages based on the event time using the Accumulator feature.

### Deploying Stream Sorter Pipeline

1. Install Numaflow.
2. Create and run a simple stream sorter pipeline
3. Verify the sorted messages in the log sink.

### Before you begin: prerequisites

To try Numaflow, you will first need to setup using one of the following options to run container images:

- [Docker Desktop](https://docs.docker.com/get-docker/)
- [podman](https://podman.io/)

Then use one of the following options to create a local Kubernetes Cluster:

- [Docker Desktop Kubernetes](https://docs.docker.com/desktop/kubernetes/)
- [k3d](https://k3d.io/)
- [kind](https://kind.sigs.k8s.io/)
- [minikube](https://minikube.sigs.k8s.io/docs/start/)

You will also need `kubectl` to manage the cluster. [Follow these steps to install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/). In case you need a refresher, all the `kubectl` commands used in this quick start guide can be found in the [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/).

### Installing Numaflow

Once you have completed all the prerequisites, run the following command lines to install Numaflow and start the [Inter-Step Buffer Service](./core-concepts/inter-step-buffer-service.md) that handles communication between vertices.

```shell
kubectl create ns numaflow-system
kubectl apply -n numaflow-system -f https://raw.githubusercontent.com/yhl25/stream-sorter/main/manifests/install.yaml
kubectl apply -f https://raw.githubusercontent.com/numaproj/numaflow/main/examples/0-isbsvc-jetstream.yaml
```

### Creating a simple stream sorter pipeline

As an example, we will create a `stream sorter pipeline` that contains a two source vertex to generate messages, a join vertex which is an accumulator that sorts the messages based on event time, and a sink vertex that logs the messages.

Run the command below to create the pipeline.

```shell
kubectl apply -f https://raw.githubusercontent.com/yhl25/stream-sorter/main/manifests/pipeline.yaml
```

### Verifying the sorted messages in the log sink

To verify the sorted messages, run the following command to view the logs of the sink vertex, the output messages should be sorted by event time.
NOTE: replace `stream-sorter-log-sink-xxxx` with the actual name of the sink vertex pod
```shell
kubectl logs -f stream-sorter-log-sink-xxxx
```