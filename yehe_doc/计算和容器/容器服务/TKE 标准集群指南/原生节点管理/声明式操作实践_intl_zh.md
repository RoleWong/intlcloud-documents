## kubectl 支持操作

<table class="table-striped">
<tbody>
	<tr>
		<th>CRD 类型</th>
		<th>操作项</th>
	</tr>
		<tr>
		<td rowspan="5">MachineSet</td>
		<td>创建原生节点池<br>`kubectl create -f machineset-demo.yaml`</td>
	</tr>
	<tr>
		<td>查看原生节点池列表<br>`kubectl get machineset`</td>
	</tr>
	<tr>
		<td>查看原生节点池 YAML 详情<br>`kubectl describe ms machineset-name`</td>
	</tr>
	<tr>
		<td>删除原生节点池<br>`kubectl delete ms machineset-name`</td>
	</tr>
	<tr>
		<td>扩容原生节点池<br>`kubectl scale --replicas=3 machineset/machineset-name`</td>
	</tr>
	<tr>
	  <td rowspan="3">Machine</td>
		<td>查看原生节点<br>`kubectl get  machine`</td>
	</tr>
	<tr>
		<td>查看原生节点 YAML 详情<br>`kubectl describe  ma machine-name`</td>
	</tr>
	<tr>
		<td>删除原生节点<br>`kubectl delete ma machine-name`</td>
	</tr>
  <tr>
    <td rowspan="4">HealthCheckPolicy</td>
    <td>创建故障检测自愈规则<br>`kubectl create -f demo-HealthCheckPolicy.yaml`</td>
  </tr>
  <tr>
    <td>查看故障自愈规则列表<br>`kubectl kubectl get HealthCheckPolicy`</td>
  </tr>
  <tr>
    <td>查看故障自愈规则 YAML 详情<br>`kubectl describe HealthCheckPolicy HealthCheckPolicy-name`</td>
  </tr>
  <tr>
    <td>删除故障自愈规则<br>`kubectl delete HealthCheckPolicy HealthCheckPolicy-name`</td>
  </tr>
</table>


## 通过 YAML 使用 CRD
###  MachineSet
```
apiVersion: node.tke.cloud.tencent.com/v1beta1
kind: MachineSet
spec:
  type: Native
  displayName: mstest
  replicas: 2
  autoRepair: true
  deletePolicy: Random
  healthCheckPolicyName: test-all
  instanceTypes:
  - C3.LARGE8
  subnetIDs:
  - subnet-xxxxxxxx
  - subnet-yyyyyyyy
  scaling:
    createPolicy: ZonePriority
    maxReplicas: 100
  template:
    spec:
      displayName: mtest
      runtimeRootDir: /var/lib/containerd
      unschedulable: false
      metadata:
        labels:
          key1: "val1"
          key2: "val2"
      providerSpec:
        type: Native
        value:
          instanceChargeType: PostpaidByHour
          lifecycle:
            preInit: "echo hello"
            postInit: "echo world"
          management:
            hosts:
            - Hostnames:
              - lkongtest
              IP: 22.22.22.22
            nameservers:
            - 183.60.83.19
            - 183.60.82.98
            - 8.8.8.8
          metadata:
            creationTimestamp: null
          securityGroupIDs:
          - sg-xxxxxxxx
          systemDisk:
            diskSize: 50
            diskType: CloudPremium
```

## kubectl 操作 demo
### MachineSet

1. 执行命令`kubectl create -f machineset-demo.yaml`根据上述 YAML 创建 MachineSet：
![](https://qcloudimg.tencent-cloud.cn/raw/fe738f177f96c69d537d17e7627ef7bc.png)

2. 根据命令`kubectl get machineset`查看 MachineSet np-pjrlok3w 状态。此时控制台上已出现对应节点池，节点在创建中：
![](https://qcloudimg.tencent-cloud.cn/raw/0310234f91bfb13155e1836f6a4f3660.png)

![](https://staticintl.cloudcachetci.com/yehe/backend-news/iMG7238_%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20230105112642.png)

3. 根据命令`kubectl describe machineset np-pjrlok3w`查看 MachineSet np-pjrlok3w 描述：
![](https://qcloudimg.tencent-cloud.cn/raw/175a797853bec72d6ae75bb488da8cbe.png)

4. 根据命令 `kubectl scale --replicas=2 machineset/np-pjrlok3w` 执行节点池扩缩容：
![](https://qcloudimg.tencent-cloud.cn/raw/70c9803505718e7bfd5c3716cab87b80.png)

5. 根据命令`kubectl delete ms np-pjrlok3w` 删除节点池。
![](https://qcloudimg.tencent-cloud.cn/raw/14e3bba4321675852202e24855e5eb58.png)

### Machine
1. 根据命令`kubectl get machine`查看 Machine 列表，此时控制台上已存在对应节点：
![](https://qcloudimg.tencent-cloud.cn/raw/de4d37e33c547cddd9c214ae55e093d4.png)
![](https://staticintl.cloudcachetci.com/yehe/backend-news/kFLI378_%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20230105143545.png)

2. 根据命令`kubectl describe ma np-14024r66-nv8bk`查看 Machine np-14024r66-nv8bk 描述：
![](https://qcloudimg.tencent-cloud.cn/raw/90545651c88836291914421607c0ed33.png)

3. 根据命令`kubectl delete ma np-14024r66-nv8bk` 删除节点。

>?
>- 如果没有调整节点池期望节点数而直接删除节点，节点池内会在检测到实际节点数不满足声明式节点数量，创建一个新节点加入节点池。删除节点的推荐操作方式如下：
	1. 调整期望节点数：kubectl scale --replicas=1 machineset/np-xxxxx
	2. 删除对应节点：kubectl delete machine np-xxxxxx-dtjhd

