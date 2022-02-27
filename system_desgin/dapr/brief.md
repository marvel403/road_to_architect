# dapr

## 简介
`Dapr is a portable, event-driven runtime that makes it easy for any developer to build resilient, stateless and stateful applications that run on the cloud and edge and embraces the diversity of languages and developer frameworks.`</br>
简单说就是为了让开发者能够更快更好的搞定微服务的框架。


## dapr 与 service mesh
service mesh更多关注网络，通过sidecar的方式，对服务进行网络流量代理、监控等
    
dapr更多的关注更快更简单的搭建微服务，为开发者服务
   
共同点：
 - 双向tls加密的服务间调用；
 - metric收集；
 - 分布式tracing；
 - 重试等调用控制

不同点：
 - dapr不提供流量控制，如路由或分流(ingress干)
 - dapr提供更具扩展性的tracing和metric，基于s2s innovation和pub/sub。

![avatar](https://docs.dapr.io/images/service-mesh.png)
 
## 基础概念
### [dapr service](https://docs.dapr.io/concepts/dapr-services/)

- sidecar daprd
    - 本地运行，self-hosted 模式，$HOME/.darp/bin/daprd，通过CLI运行
    k8s，运行dapr-sidecar-injector service，监听拥有 dapr.io/enabled 标签的pod，并把daprd container注入到pod
        daprd运行参数：https://docs.dapr.io/reference/arguments-annotations-overview/
- operator
    - k8s模式下，有一个专门的pod运行operator，用于管理dapr component的更新，并为dapr提供k8s的服务接口
- placement
    - k8s模式、self-hosted模式，用于 compute和distribute 分布式哈希表，哈希表用于管理actors的id 到 pod或process的映射
- sentry
    - 管理service之间的mtls，承担CA中心的角色。
- sidecar injector
    - k8s模式，有一个专门的pod运行injector，监听初始化了dapr标签的pod，并在pod中启动daprd 

### [building block](https://docs.dapr.io/concepts/building-blocks-concept/)
- 由一个或多个component组成，并开放接口(http/grpc)给业务代码调用
    - Service-to-service invocation：服务间调用。提供反向代理、服务发现
    - State management：状态管理。提供持久化的、可插拔的、kv对状态管理。
    - Publish and subscribe
    - Resource bindings：可以连接外部的服务
    - Actors
    - Observability
    - Secrets
    - Configuration

### [components](https://docs.dapr.io/concepts/components-concept/)
会被building block和application使用到的模块化功能
 - state store：状态存储，kv数据存储
 - name resolution：服务发现
 - pub/sub brokers：发布订阅服务端
 - bindings：资源绑定
 - secret stores：秘钥存储
 - configuration stores：配置存储
 - middleware：允许开发者自定义中间件，在http请求中附加处理。如：认证、加解密等。

