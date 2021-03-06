

# ULB：负载均衡类型/网络模式

## 负载均衡类型
负载均衡类型|支持协议
:-:|:-
请求代理型|HTTP、HTTPS、TCP
报文转发型|TCP、UDP

历史创建的实例为兼容型，可同时包含请求代理型、报文转发型的VServer。

### 请求代理型负载均衡TCP和HTTP差异
TCP：接收请求，选择后端节点，连接后端节点，转发内容；可以将上层其他协议的报文直接转发至后端服务节点。

HTTP：接收请求，解析请求，根据转发规则选择服务节点集群，根据ULB算法选择后端服务节点，连接服务节点，接收响应，解析响应头，添加适当的响应头（如Set-cookie等），返回响应内容给客户端。

### 请求代理型TCP和报文转发型TCP的差异
请求代理：需要维护客户端到ULB和ULB到后端服务节点的两个TCP连接（需要经历两次TCP握手）。

报文转发：只需要对报文的解析和转发，少去了连接建立的开销，报文转发的效率高于请求代理模式多个数量级，但具有以下限制：

*  ULB只会修改目的MAC地址，不支持后端服务节点监听不同的端口，如果监听端口与服务接收端口不一致，会导致数据传输出错。

*  后端服务节点必须配置ULB的服务IP地址。

如无在一个服务节点上监听多个端口的需求，则可选择报文转发模式，转发性能占优。

## 网络模式

### 外网ULB

外网ULB，对外提供服务的IP地址为外网EIP，用于接收来自Internet的客户端请求。若需要ULB转发外网请求，创建ULB时选择“外网”。

对于EIP，需要根据业务情况选择带宽、计费模式等属性。详见[EIP简介](https://docs.ucloud.cn/unet/eip/introduction)。

### 内网ULB

内网ULB，对外提供服务的IP地址为内网IP，用于接收内网的客户端请求。若需要ULB转发内网请求，创建ULB时选择“内网”。内网IP地址将从选择的子网中分配。

