# WIP

### Why

-  It can help us improve the overall quality and reliability of our Kubernetes configurations. 
It is a tool used for evaluating the security and best practices of Kubernetes YAML files.

### Test

1. Check a simple yaml file of the nginx pod
  - ```docker run -v $(pwd):/project zegl/kube-score:latest score pod-example.yaml```

    ```
     v1/Pod nginx                                                                  💥
    [CRITICAL] Pod NetworkPolicy
        · The pod does not have a matching NetworkPolicy
            Create a NetworkPolicy that targets this pod to control who/what
            can communicate with this pod. Note, this feature needs to be
            supported by the CNI implementation used in the Kubernetes cluster
            to have an effect.
    [CRITICAL] Container Security Context User Group ID
        · nginx -> Container has no configured security context
            Set securityContext to run the container in a more secure context.
    [CRITICAL] Container Ephemeral Storage Request and Limit
        · nginx -> Ephemeral Storage limit is not set
            Resource limits are recommended to avoid resource DDOS. Set
            resources.limits.ephemeral-storage
    [CRITICAL] Container Security Context ReadOnlyRootFilesystem
        · nginx -> Container has no configured security context
            Set securityContext to run the container in a more secure context.
    [CRITICAL] Container Image Tag
        · nginx -> Image with latest tag
            Using a fixed tag is recommended to avoid accidental upgrades
    ```
### Resources

1* https://github.com/zegl/kube-score/tree/master
