Windows10上安装MiniKubernetes
---

# 1. 安装VirtualBox
1. 官网:https://www.virtualbox.org/wiki/Downloads
2. 下载对应的Windows的Exe安装到本地即可
3. 可以通过打开这个虚拟机管理器来查看目前的虚拟机

# 2. 安装MiniKube
1. MiniKube的文档:https://minikube.sigs.k8s.io/docs/start/
2. 安装kubectl的文档:https://kubernetes.io/docs/tasks/tools/install-kubectl/
   1. 查看其中的Windows，下载Kubectl.exe
   2. 配置环境变量
      1. 添加KUBECRL变量为存放kuberctl.exe的路径
      2. 在系统变量PATH中添加%KUBECRL%(或者绝对路径)

# 3. 检查安装情况
1. 不翻墙的启动
```
minikube start --vm-driver=virtualbox --registry-mirror=https://ytsch4ca.mirror.aliyuncs.com --image-repository=registry.aliyuncs.com/google_containers --iso-url=https://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/iso/minikube-v1.8.0.iso
```

# 4. 命令

## 4.1. Minikube
1. `minikube status`:查看状态
2. `minikube start`:启动
3. `minikube stop`:停止集群
4. `minikube dashboard`:启动可视化面板
5. `minikube ssh`:登录进入Linux虚拟机

## 4.2. Kubectl
1. `kubectl get nodes`:查看节点的情况，确认POD的启动请情况

### 4.2.1. 部署一下2048应用
2. `kubectl create deployment hello-minikube --image=daocloud.io/daocloud/dao-2048:latest`:创建一个Kubernetes的Deployment 
3. `kubectl expose deployment hello-minikube --type=NodePort --port=80`:将服务进行暴露，通过80端口暴露
4. 获取暴露的Service的URL来查看:`minikube service hello-minikube --url`，之后可以通过接口访问

# 5. 参考文档
1. <a href = "https://blog.csdn.net/bruceRong/article/details/84942225">Kubernates的安装和使用</a>