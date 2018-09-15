Docker Swarm 管理的是 Docker Host 集群。
node

swarm 中的每个 Docker Engine 都是一个 node，有两种类型的 node：manager 和 worker。

为了向 swarm 中部署应用，我们需要在 manager node 上执行部署命令，manager node 会将部署任务拆解并分配给一个或多个 worker node 完成部署。

manager node 负责执行编排和集群管理工作，保持并维护 swarm 处于期望的状态。swarm 中如果有多个 manager node，它们会自动协商并选举出一个 leader 执行编排任务。

woker node 接受并执行由 manager node 派发的任务。默认配置下 manager node 同时也是一个 worker node，不过可以将其配置成 manager-only node，让其专职负责编排和集群管理工作。

work node 会定期向 manager node 报告自己的状态和它正在执行的任务的状态，这样 manager 就可以维护整个集群的状态。

service

service 定义了 worker node 上要执行的任务。swarm 的主要编排任务就是保证 service 处于期望的状态下。

举一个 service 的例子：在 swarm 中启动一个 http 服务，使用的镜像是 httpd:latest，副本数为 3。

manager node 负责创建这个 service，经过分析知道需要启动 3 个 httpd 容器，根据当前各 worker node 的状态将运行容器的任务分配下去，比如 worker1 上运行两个容器，worker2 上运行一个容器。

运行了一段时间，worker2 突然宕机了，manager 监控到这个故障，于是立即在 worker3 上启动了一个新的 httpd 容器。

这样就保证了 service 处于期望的三个副本状态。
