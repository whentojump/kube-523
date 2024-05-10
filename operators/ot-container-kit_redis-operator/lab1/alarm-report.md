## Summary
Essentially all the alarms generated by Acto for this operator are simple misoperations. The vulnerability of this operator to misoperations seems to be understood and is documented in this github issue: https://github.com/OT-CONTAINER-KIT/redis-operator/issues/490

# ALARM 1
trial-00-0059/0001

## What happened
When acto adds a sidecar with invalid parameters to the spec, a pod crashes.

## Root Cause
The spec does not specify a valid sidecar. The operator code attempts to update the `SatetfullSet`  with the updated spec in `k8sutils/satefulset.go` without performing any validation on the statefulset spec. The sidecar image in the spec is not valid so when the cluster restarts it repeatedly fails as it is unable to create a pod with the invalid sidecar spec.

## Expected behavior?
This is a misoperation. The operator should be able to easily reject the new spec as the sidecar spec does not specify a valid container

# ALARM 2
trial-02-0059/0001

## What happened
Same as ALARM 1 except with an array of invalid sidecars.

# ALARM 3
trial-02-0059/0001

## What happened
When acto adds a TLS configuration with invalid parameters to the spec, a pod crashes.

## Root Cause
The spec does not specify a valid TLS cert. The operator code attempts to update the `SatetfullSet`  with the updated spec in `k8sutils/satefulset.go` without performing any validation on the statefulset spec. When the cluster restarts it repeatedly fails as it is unable to create a pod with the invalid TLS spec

## Expected behavior?
This is a misoperation. The operator should be able to easily reject the new spec as the TLS spec does not specify a valid cert.

# ALARM 4
trial-01-0042/0001

## What happened
Same as ALARM 3.

# ALARM 5
trial-02-0040/0001

## What happened
Same as ALARM 3.

# ALARM 6
trial-00-0051/0001

## What happened
When acto adds a `RedisSecret` with "ACTOKEY" as the key, the pod enters an unhealthy state.

## Root Cause
The operator assumes that the `RedisSecret` field will specify a password for Redis. As such it assumes that the value of key will be "password" and that the name will be a valid password. The operator only checks if `RedisSecret` has been defined and does not check if name as been specified. As such when the operator attempts to create a pod with the new spec, it fails when it tries to get the name that is not specified. For example at: https://github.com/OT-CONTAINER-KIT/redis-operator/blob/891d740293ab27ee8502f212088d05e0b29cb15f/k8sutils/redis.go#L113.

## Expected behavior?
This is a misoperation. The operator should be able to easily reject the spec as the password is not properly specified. 


# ALARM 7
trial-01-0044/0001

## What happened
Same as ALARM 6.

# ALARM 8
trial-01-0045/0001

## What happened
Same as ALARM 6.

# ALARM 9
trial-00-0051/0001

## What happened
When acto adds a `service` with "ClusterIP" as the `serviceType`, the pod enters an unhealthy state.

## Root Cause
The operator assumes that the `service` will specify a valid service, either a load balancer, node port, or cluster IP. However, it does not check if all the fields are specified or if they are valid. In this test case since only the `serviceType` is specified, the operator fails to create a pod with the new spec.

## Expected behavior?
This is a misoperation. The operator should be able to easily reject the spec as the service is not properly defined. 

# ALARM 10
trial-01-0045/0001

## What happened
Similar to ALARM 9. Only `annotation` is specified within `service`.

# ALARM 11
trial-01-0025/0001

## What happened
Similar to ALARM 9. Only `annotation` is invalid.

# ALARM 12
trial-01-0026/0001

## What happened
Same as ALARM 11.

# ALARM 13
trial-01-0031/0001

## What happened
Same as ALARM 11.

# ALARM 14
trial-01-0023/0001

## What happened
Same as ALARM 11.

# ALARM 15
trial-03-0005/0001

## What happened
Same as ALARM 9

# ALARM 16
trial-03-0006/0001

## What happened
Similar to ALARM 9 but "LoadBalancer" is specified instead.

# ALARM 17
trial-03-0007/0001

## What happened
Similar to ALARM 9 but "NodePort" is specified instead.

# ALARM 18
trial-03-0008/0001

## What happened
Same as alarm 9.

# ALARM 19
trial-00-0003/0001

## What happened
When acto adds a RedisLeader configuration but specifies invalid values for "Tolerations" in the spec, a pod crashes.

## Root Cause
The spec does specifies invalid values such as "ACTOKEY" for each of the fields. The operator code attempts to update the `SatetfullSet`  with the updated spec in `k8sutils/satefulset.go` without performing any validation on the statefulset spec. When the cluster restarts it repeatedly fails as it is unable to create a pod with the invalid RedisLeader spec.

## Expected behavior?
This is a misoperation. The operator should be able to perform some validation on the tolerations before accepting the spec. For example, the "operator" and "effect" fields could be enums to avoid invalid values.

# ALARM 20
trial-00-0004/0001

## What happened
Similar to ALARM 19. But none of the necesary fields for "tolerations" are specified.

# ALARM 21
trial-00-0005/0001

## What happened
Same as ALARM 19.

# ALARM 22
trial-00-0006/0001

## What happened
Same as ALARM 20.

# ALARM 23
trial-00-0020/0001

## What happened
Same as ALARM 19.

# ALARM 24
trial-00-0053/0001

## What happened
Same as ALARM 19.

# ALARM 25
trial-00-0054/0001

## What happened
Same as ALARM 19.

# ALARM 26
trial-01-0011/0001

## What happened
Same as ALARM 19.

# ALARM 27
trial-01-0012/0001

## What happened
Same as ALARM 19.

# ALARM 28
trial-01-0038/0001

## What happened
Same as ALARM 19.

# ALARM 29
trial-01-0039/0001

## What happened
Same as ALARM 20.

# ALARM 30
trial-01-0040/0001

## What happened
Same as ALARM 19.

# ALARM 31
trial-01-0041/0001

## What happened
Same as ALARM 20.

# ALARM 32
trial-03-0048/0001

## What happened
Same as ALARM 19.

# ALARM 33
trial-00-0023/0001

## What happened
When acto adds "initContainer" options with invalid parameters to the spec, a pod crashes.

## Root Cause
The spec does not specify a valid image for the initialization options. The operator code attempts to update the `SatetfullSet`  with the updated spec in `k8sutils/satefulset.go` without performing any validation on the statefulset spec. The pod then crashes when it tries to reconcile the state as it cannot find the requested image.

## Expected behavior?
This is a misoperation. The operator should be able to easily reject the new spec does not specify a valid container

# ALARM 34
trial-00-0024/0001

## What happened
Same as ALARM 33.

# ALARM 35
trial-00-0025/0001

## What happened
Same as ALARM 33.

# ALARM 36
trial-00-0026/0001

## What happened
Same as ALARM 33.

# ALARM 37
trial-01-0001/0001

## What happened
Same as ALARM 33.

# ALARM 38
trial-01-0002/0001

## What happened
Same as ALARM 33.

# ALARM 39
trial-01-0003/0001

## What happened
Same as ALARM 33.

# ALARM 40
trial-01-0004/0001

## What happened
Same as ALARM 33.

# ALARM 41
trial-02-0014/0001

## What happened
Same as ALARM 33.

# ALARM 42
trial-02-0015/0001

## What happened
Same as ALARM 33.

# ALARM 43
trial-02-0016/0001

## What happened
Same as ALARM 33.

# ALARM 44
trial-02-0028/0001

## What happened
Same as ALARM 33.

# ALARM 45
trial-02-0300/0001

## What happened
Same as ALARM 33.

# ALARM 46
trial-02-0031/0001

## What happened
Same as ALARM 33.

# ALARM 47
trial-02-0032/0001

## What happened
Same as ALARM 33.

# ALARM 48
trial-02-0047/0001

## What happened
Same as ALARM 33.

# ALARM 49
trial-02-0048/0001

## What happened
Same as ALARM 33.

# ALARM 50
trial-02-0049/0001

## What happened
Same as ALARM 33.

# ALARM 51
trial-03-0018/0001

## What happened
Same as ALARM 33.

# ALARM 52
trial-03-0019/0001

## What happened
Same as ALARM 33.

# ALARM 53
trial-03-0053/0001

## What happened
Same as ALARM 33.

# ALARM 54
trial-03-0054/0001

## What happened
Same as ALARM 33.

# ALARM 55
trial-00-0062/0001

## What happened
Same as ALARM 1.

# ALARM 56
trial-01-0054/0001

## What happened
Same as ALARM 1.

# ALARM 57
trial-01-0055/0001

## What happened
Same as ALARM 1.

# ALARM 58
trial-01-0056/0001

## What happened
Same as ALARM 1.

# ALARM 59
trial-03-0000/0001

## What happened
Same as ALARM 1.

# ALARM 60
trial-03-0001/0001

## What happened
Same as ALARM 1.

# ALARM 61
trial-03-0027/0001

## What happened
Same as ALARM 1.

# ALARM 62
trial-03-0028/0001

## What happened
Same as ALARM 1.

# ALARM 63
trial-03-0050/0001

## What happened
Same as ALARM 1.

# ALARM 64
trial-03-0051/0001

## What happened
Same as ALARM 1.

# ALARM 65
trial-03-0052/0001

## What happened
Same as ALARM 1.

# ALARM 66
trial-00-0016/0001

## What happened
When acto adds a resource limit on a resource that does not exist on the cluster the pod enters an unhealthy state.

## Root Cause
The spec does not specify a valid sidecar. The operator code attempts to update the `SatetfullSet`  with the updated spec in `k8sutils/satefulset.go` without performing any validation on the statefulset spec. The requested resource limit is not valid so when the cluster restarts it repeatedly fails as it is unable to create a pod as it cannot find the resource.

## Expected behavior?
This is a misoperation. The operator should be able to reject the spec by checking if the limit is on a resource that actually exists in the cluster.

# ALARM 67
trial-00-0017/0001

## What happened
Similar to ALARM 66 but the resource limit is specified for a sidecar.

# ALARM 68
trial-02-0054/0001

## What happened
Same as ALARM 67.

# ALARM 69
trial-03-0038/0001

## What happened
Same as ALARM 67.

# ALARM 70
trial-00-0027/0001

## What happened
When acto sets `cr.spec.kubernetesConfig.imagePullSecret` to "INVALID_NAME" or some other invalid value which causes to pod to crash.

## Root Cause
The value for `imagePullSecret` is not valid. The operator code attempts to update the `SatetfullSet`  with the updated spec in `k8sutils/satefulset.go` without performing any validation on the statefulset spec. As the value is not valid the pod crashes when it tries to reconcile the state.

## Expected behavior?
This is a misoperation. The operator should handle this failure more gracefully. For example, after finding that the `imagePullSecret` value is invalid it should revert to the default value.

# ALARM 71
trial-00-0035/0001

## What happened
Same as ALARM 70.

# ALARM 72
trial-00-0036/0001

## What happened
Same as ALARM 70.

# ALARM 73
trial-00-0037/0001

## What happened
Same as ALARM 70.

# ALARM 74
trial-00-0037/0001

## What happened
Same as ALARM 70.

# ALARM 75
trial-03-0018/0001

## What happened
Similar to ALARM 70 but `cr.spec.kubernetesConfig.imagePullPolicy` is specified to an invalid value instead.

# ALARM 76
trial-03-0019/0001

## What happened
Same as ALARM 75.

# ALARM 77
trial-01-0049/0001

## What happened
When acto sets `cr.spec.nodeSelector` to to select a node that does not exist in the cluster, the pod enters an unhealthy state.

## Root Cause
Acto does not perform any validation to determine if the selected node exists. As such the operator code attempts to update the `SatetfullSet`  with the updated spec in `k8sutils/satefulset.go` without performing any validation on the statefulset spec. As the node does not exist the pod crashes when it tries to reconcile the state.

## Expected behavior?
This is a misoperation. The operator should check if the node exists before accepting the spec.

# ALARM 78
trial-01-0050/0001

## What happened
Same as ALARM 77.

# ALARM 79
trial-00-0001/0001

## What happened
When acto adds a storage volume claim using a resource that does not exist in the cluster, the pod enters an unhealthy state.

## Root Cause
The spec does not specify a valid sidecar. The operator code attempts to update the `SatetfullSet`  with the updated spec in `k8sutils/satefulset.go` without performing any validation on the statefulset spec. The requested resources under `nodeConfVolumeClaimTemplate` does not exist so when the cluster restarts it repeatedly fails as it is unable to create a pod as it cannot find the resource.

## Expected behavior?
This is a misoperation. The operator should be able to reject the spec by checking if the storage resource actually exists.

# ALARM 80
trial-00-0002/0001

## What happened
Similar to ALARM 79 but but the spec under `volumeMount` specifies a resource that does not exist.

# ALARM 81
trial-00-0008/0001

## What happened
Same as ALARM 79.

# ALARM 82
trial-00-0009/0001

## What happened
Same as ALARM 79.

# ALARM 83
trial-00-0050/0001

## What happened
Same as ALARM 79.

# ALARM 84
trial-01-0000/0001

## What happened
Same as ALARM 79.

# ALARM 85
trial-01-0007/0001

## What happened
Same as ALARM 79.

# ALARM 86
trial-02-0020/0001

## What happened
Same as ALARM 79.

# ALARM 87
trial-02-0021/0001

## What happened
Same as ALARM 79.

# ALARM 88
trial-00-0012/0001

## What happened
Same as ALARM 80.

# ALARM 89
trial-00-0013/0001

## What happened
Same as ALARM 80.

# ALARM 90
trial-00-0014/0001

## What happened
Same as ALARM 80.

# ALARM 91
trial-00-0015/0001

## What happened
Same as ALARM 80.

# ALARM 92
trial-00-0022/0001

## What happened
Same as ALARM 80.

# ALARM 93
trial-00-0049/0001

## What happened
Same as ALARM 80.

# ALARM 94
trial-00-0056/0001

## What happened
Similar to ALARM 80 but the spec under `volumeMount` specifies an invalid path.

# ALARM 95
trial-01-0028/0001

## What happened
Same as ALARM 94.

# ALARM 96
trial-01-0048/0001

## What happened
Same as ALARM 80.

# ALARM 97
trial-01-0049/0001

## What happened
Same as ALARM 80.

# ALARM 98
trial-02-0007/0001

## What happened
Same as ALARM 80.

# ALARM 99
trial-03-0013/0003

## What happened
Similar to 80 but `volumeMount` is an empty array so it is invalid.

# ALARM 100
trial-03-0023/0001

## What happened
Same as 80