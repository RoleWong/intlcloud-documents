### 如何禁止 Pod 调度到某个虚拟节点？

默认情况下，TKE 普通集群添加了虚拟节点节点池后，会在 Node 资源不足时，自动向虚拟节点调度 Pod。弹性集群则会自动在多个虚拟节点随机调度 Pod。

此时如果您不希望向某虚拟节点（代表某子网/可用区）调度，可通过以下两种方式封锁虚拟节点实现禁止调度：
- 通过 [容器服务控制台](https://console.cloud.tencent.com/tke2/cluster) 对节点进行**封锁**操作，详情可参见 [封锁节点](https://intl.cloud.tencent.com/document/product/457/30654)。
- 通过命令行执行如下命令实现禁止调度：
```plaintext
$kubectl cordon $虚拟节点名称
```

### 如何禁止 TKE 普通集群在资源不足时自动调度到虚拟节点？


需要在 kube-system 命名空间下，新建名为 eks-config 的 configmap。执行命令如下：

```plaintext
$kubectl create configmap eks-config --from-literal=AUTO_SCALE_EKS=false
```
将 AUTO_SCALE_EKS 的 value 设置为 false，即可关闭 TKE 普通集群向虚拟节点的自动调度机制。


### 如何手动调度 Pod 到虚拟节点？[](id:pod1)

默认虚拟节点会自动添加 Taints 以降低调度优先级，如需手动调度 Pod 到虚拟节点或指定虚拟节点，通常需要为 Pod 添加对应的 Tolerations。但并非所有的 Pod 均可以调度到虚拟节点上，详情请参见 [虚拟节点调度说明](https://intl.cloud.tencent.com/document/product/457/39760)。为方便使用，您可以在 Pod Spec 中指定 nodeselector 。示例如下：

```yaml
spec:    
    nodeSelector:
      node.kubernetes.io/instance-type: eklet
```

或在 Pod Spec 指定 nodename。示例如下：
```yaml
spec:
   nodeName: $虚拟节点名称
```

TKE 的管控组件会判断该 Pod 是否可以调度到虚拟节点，若不支持则不会调度到虚拟节点。



### 如何强制调度 Pod 到虚拟节点，无论虚拟节点是否支持该 Pod？

>!强制调度到虚拟节点，Pod 可能会创建失败，失败原因可参见 [如何手动调度 Pod 到虚拟节点](#pod1)。

如需强制调度 Pod 到虚拟节点，除了为 Pod 指定 nodeselector 或 nodename，也必须添加对应的 Tolerations。

1. 为 Pod Spec 中指定 nodeselector。示例如下：
```yaml
 spec:    
      nodeSelector:
        node.kubernetes.io/instance-type: eklet
```
 或在 Pod Spec 指定 nodename。示例如下：
```yaml
spec: 
      nodeName: $虚拟节点名称
```

2. 为 Pod 添加 Tolerations。示例如下：
```yaml
spec: 
      tolerations: 
      - effect: NoSchedule
        key: eks.tke.cloud.tencent.com/eklet
        operator: Exists
```



### 如何自定义虚拟节点 DNS？
弹性容器服务 EKS 支持虚拟节点特性，您可通过在 yaml 中定义 annotation 的方式，实现自定义 DNS 等能力。详情可参见 [虚拟节点 annotation 说明](https://intl.cloud.tencent.com/document/product/457/36162)。
