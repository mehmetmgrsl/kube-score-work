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

2. Check the pod and its service yaml files together:
  - ```docker run -v $(pwd):/project zegl/kube-score:latest score *.yaml```

    ```
    v1/Pod nginx                                                          💥
    [CRITICAL] Container Security Context User Group ID
        · nginx -> Container has no configured security context
            Set securityContext to run the container in a more secure context.
    [CRITICAL] Pod NetworkPolicy
        · The pod does not have a matching NetworkPolicy
            Create a NetworkPolicy that targets this pod to control who/what
            can communicate with this pod. Note, this feature needs to be
            supported by the CNI implementation used in the Kubernetes cluster
            to have an effect.
    [CRITICAL] Container Ephemeral Storage Request and Limit
        · nginx -> Ephemeral Storage limit is not set
            Resource limits are recommended to avoid resource DDOS. Set
            resources.limits.ephemeral-storage
    [CRITICAL] Container Image Tag
        · nginx -> Image with latest tag
            Using a fixed tag is recommended to avoid accidental upgrades
    [CRITICAL] Pod Probes
        · Container is missing a readinessProbe
            A readinessProbe should be used to indicate when the service is
            ready to receive traffic. Without it, the Pod is risking to receive
            traffic before it has booted. It's also used during rollouts, and
            can prevent downtime if a new version of the application is failing.
            More information: https://github.com/zegl/kube-score/blob/master/README_PROBES.md
    [CRITICAL] Container Security Context ReadOnlyRootFilesystem
        · nginx -> Container has no configured security context
            Set securityContext to run the container in a more secure context.
v1/Service nginx-service                                                      🤔
    [WARNING] Service Type
        · The service is of type NodePort
            NodePort services should be avoided as they are insecure, and can
            not be used together with NetworkPolicies. LoadBalancers or use of
            an Ingress is recommended over NodePorts.
    ```
### Resources

1* https://github.com/zegl/kube-score/tree/master
