# Nacos

## 一、Nacos简介

### 1.概况

Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。

Nacos 帮助您更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。

### 2.什么是 Nacos？

Nacos 是阿里巴巴推出来的一个新开源项目，这是一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。

Nacos 如果要和SpringCloud的一个组件最对比的话，毋庸置疑，作用一定是和注册中心Eureka是一样的，但是我这里为什么称Nacos为服务中心，因为Nacos更偏向于服务，Nacos除了和Eureka一样有服务的注册和发现意外，同时还拥有了配置管理，服务上下线等功能的配置。

Nacos 致力于发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。

Nacos 更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。

#### （1）服务发现和服务健康监测

Nacos 支持基于 DNS 和基于 RPC 的服务发现。服务提供者使用 [原生SDK](https://nacos.io/zh-cn/docs/sdk.html)、[OpenAPI](https://nacos.io/zh-cn/docs/open-API.html)、或一个[独立的Agent TODO](https://nacos.io/zh-cn/docs/other-language.html)注册 Service 后，服务消费者可以使用[DNS TODO](https://nacos.io/zh-cn/docs/xx) 或[HTTP&API](https://nacos.io/zh-cn/docs/open-API.html)查找和发现服务。

Nacos 提供对服务的实时的健康检查，阻止向不健康的主机或服务实例发送请求。Nacos 支持传输层 (PING 或 TCP)和应用层 (如 HTTP、MySQL、用户自定义）的健康检查。 对于复杂的云环境和网络拓扑环境中（如 VPC、边缘网络等）服务的健康检查，Nacos 提供了 agent 上报模式和服务端主动检测2种健康检查模式。Nacos 还提供了统一的健康检查仪表盘，帮助您根据健康状态管理服务的可用性及流量。

#### （2）动态配置服务

动态配置服务可以让您以中心化、外部化和动态化的方式管理所有环境的应用配置和服务配置。

动态配置消除了配置变更时重新部署应用和服务的需要，让配置管理变得更加高效和敏捷。

配置中心化管理让实现无状态服务变得更简单，让服务按需弹性扩展变得更容易。

Nacos 提供了一个简洁易用的UI ([控制台样例 Demo](http://console.nacos.io/nacos/index.html)) 帮助您管理所有的服务和应用的配置。Nacos 还提供包括配置版本跟踪、金丝雀发布、一键回滚配置以及客户端配置更新状态跟踪在内的一系列开箱即用的配置管理特性，帮助您更安全地在生产环境中管理配置变更和降低配置变更带来的风险。

#### （3）动态 DNS 服务

动态 DNS 服务支持权重路由，让您更容易地实现中间层负载均衡、更灵活的路由策略、流量控制以及数据中心内网的简单DNS解析服务。动态DNS服务还能让您更容易地实现以 DNS 协议为基础的服务发现，以帮助您消除耦合到厂商私有服务发现 API 上的风险。

Nacos 提供了一些简单的 [DNS APIs TODO](https://nacos.io/zh-cn/docs/xx) 帮助您管理服务的关联域名和可用的 IP:PORT 列表.

（4）服务及其元数据管理

Nacos 能让您从微服务平台建设的视角管理数据中心的所有服务及元数据，包括管理服务的描述、生命周期、服务的静态依赖分析、服务的健康状态、服务的流量管理、路由及安全策略、服务的 SLA 以及最首要的 metrics 统计数据。

### 3.Nacos 地图

![nacos_map](https://nacos.io/img/nacosMap.jpg)



## 二、Nacos中的概念

**地域**：物理的数据中心，资源创建成功后不能更换。

**可用区**：同一地域内，电力和网络互相独立的物理区域。同一可用区内，实例的网络延迟较低。

**接入点**：地域的某个服务的入口域名。

**命名空间**：用于进行租户粒度的配置隔离。不同的命名空间下，可以存在相同的 Group 或 Data ID 的配置。Namespace 的常用场景之一是不同环境的配置的区分隔离，例如开发测试环境和生产环境的资源（如配置、服务）隔离等。

**配置**：在系统开发过程中，开发者通常会将一些需要变更的参数、变量等从代码中分离出来独立管理，以独立的配置文件的形式存在。目的是让静态的系统工件或者交付物（如 WAR，JAR 包等）更好地和实际的物理运行环境进行适配。配置管理一般包含在系统部署的过程中，由系统管理员或者运维人员完成。配置变更是调整系统运行时的行为的有效手段。

**配置管理**：系统配置的编辑、存储、分发、变更管理、历史版本管理、变更审计等所有与配置相关的活动。

**配置项**：一个具体的可配置的参数与其值域，通常以 param-key=param-value 的形式存在。例如我们常配置系统的日志输出级别（logLevel=INFO|WARN|ERROR） 就是一个配置项。

**配置集**：一组相关或者不相关的配置项的集合称为配置集。在系统中，一个配置文件通常就是一个配置集，包含了系统各个方面的配置。例如，一个配置集可能包含了数据源、线程池、日志级别等配置项。

**配置集 ID**：Nacos 中的某个配置集的 ID。配置集 ID 是组织划分配置的维度之一。Data ID 通常用于组织划分系统的配置集。一个系统或者应用可以包含多个配置集，每个配置集都可以被一个有意义的名称标识。Data ID 通常采用类 Java 包（如 com.taobao.tc.refund.log.level）的命名规则保证全局唯一性。此命名规则非强制。

**配置分组**：Nacos 中的一组配置集，是组织配置的维度之一。通过一个有意义的字符串（如 Buy 或 Trade ）对配置集进行分组，从而区分 Data ID 相同的配置集。当您在 Nacos 上创建一个配置时，如果未填写配置分组的名称，则配置分组的名称默认采用 DEFAULT_GROUP 。配置分组的常见场景：不同的应用或组件使用了相同的配置类型，如 database_url 配置和 MQ_topic 配置。

**配置快照**：Nacos 的客户端 SDK 会在本地生成配置的快照。当客户端无法连接到 Nacos Server 时，可以使用配置快照显示系统的整体容灾能力。配置快照类似于 Git 中的本地 commit，也类似于缓存，会在适当的时机更新，但是并没有缓存过期（expiration）的概念。

**服务**：通过预定义接口网络访问的提供给客户端的软件功能。

**服务名**：服务提供的标识，通过该标识可以唯一确定其指代的服务。

**服务注册中心**：存储服务实例和服务负载均衡策略的数据库。

**服务发现**：在计算机网络上，（通常使用服务名）对服务下的实例的地址和元数据进行探测，并以预先定义的接口提供给客户端进行查询。

**元信息**：Nacos数据（如配置和服务）描述信息，如服务版本、权重、容灾策略、负载均衡策略、鉴权配置、各种自定义标签 (label)，从作用范围来看，分为服务级别的元信息、集群的元信息及实例的元信息。

**应用**：用于标识服务提供方的服务的属性。

**服务分组**：不同的服务可以归类到同一分组。

**虚拟集群**：同一个服务下的所有服务实例组成一个默认集群, 集群可以被进一步按需求划分，划分的单位可以是虚拟集群。

**实例**：提供一个或多个服务的具有可访问网络地址（IP:Port）的进程。

**权重**：实例级别的配置。权重为浮点数。权重越大，分配给该实例的流量越大。

**健康检查**：以指定方式检查服务下挂载的实例 (Instance) 的健康度，从而确认该实例 (Instance) 是否能提供服务。根据检查结果，实例 (Instance) 会被判断为健康或不健康。对服务发起解析请求时，不健康的实例 (Instance) 不会返回给客户端。

**健康保护阈值**：为了防止因过多实例 (Instance) 不健康导致流量全部流向健康实例 (Instance) ，继而造成流量压力把健康 健康实例 (Instance) 压垮并形成雪崩效应，应将健康保护阈值定义为一个 0 到 1 之间的浮点数。当域名健康实例 (Instance) 占总服务实例 (Instance) 的比例小于该值时，无论实例 (Instance) 是否健康，都会将这个实例 (Instance) 返回给客户端。这样做虽然损失了一部分流量，但是保证了集群的剩余健康实例 (Instance) 能正常工作。



## 三、Nacos原理

### 1.nacos简单介绍

![img](https://upload-images.jianshu.io/upload_images/18465434-6e52eea204e1dd68.png?imageMogr2/auto-orient/strip|imageView2/2/w/804/format/webp)

Nacos注册中心分为server与client，server采用Java编写，为client提供注册发现服务与配置服务。而client可以用多语言实现，client与微服务嵌套在一起，nacos提供sdk和openApi，如果没有sdk也可以根据openApi手动写服务注册与发现和配置拉取的逻辑。

### 2.nacos服务领域模型

![img](https://upload-images.jianshu.io/upload_images/18465434-4969f32b90966ec1.png?imageMogr2/auto-orient/strip|imageView2/2/w/432/format/webp)

Nacos服务领域模型主要分为命名空间、集群、服务。在下图的分级存储模型可以看到，在服务级别，保存了健康检查开关、元数据、路由机制、保护阈值等设置，而集群保存了健康检查模式、元数据、同步机制等数据，实例保存了该实例的ip、端口、权重、健康检查状态、下线状态、元数据、响应时间。

![img](https://upload-images.jianshu.io/upload_images/18465434-755eec44672473e9.png?imageMogr2/auto-orient/strip|imageView2/2/w/632/format/webp)



### 3.注册中心原理

![img](https://upload-images.jianshu.io/upload_images/18465434-d17f9ad8f6655a0a.png?imageMogr2/auto-orient/strip|imageView2/2/w/814/format/webp)

服务注册方法：以Java nacos client v1.0.1  为例子，服务注册的策略的是每5秒向nacos server发送一次心跳，心跳带上了服务名，服务ip，服务端口等信息。同时 nacos server也会向client 主动发起健康检查，支持tcp/http检查。如果15秒内无心跳且健康检查失败则认为实例不健康，如果30秒内健康检查失败则剔除实例。

### 4.配置中心原理

![img](https://upload-images.jianshu.io/upload_images/18465434-28cf0f9e370f5b64.png?imageMogr2/auto-orient/strip|imageView2/2/w/575/format/webp)



## 四、 Nacos简单搭建

### **1.环境准备**

Nacos 依赖 Java 环境来运行。如果您是从代码开始构建并运行Nacos，还需要为此配置 Maven环境，请确保是在以下版本环境中安装使用:

64 bit OS，支持 Linux/Unix/Mac/Windows，推荐选用 Linux/Unix/Mac。
64 bit JDK 1.8+。
Maven 3.2.x+。

### 2.下载源码或者安装包

可以通过源码和发行包两种方式来获取 Nacos。

#### （1）从 Github 上下载源码方式

```java
git clone https://github.com/alibaba/nacos.git
cd nacos/
mvn -Prelease-nacos clean install -U  
ls -al distribution/target/

// change the $version to your actual path
cd distribution/target/nacos-server-$version/nacos/bin
```

#### （2）下载编译后压缩包方式

可以从 [最新稳定版本](https://github.com/alibaba/nacos/releases) 下载 nacos-server-$version.zip 包。

git官方地址下载nacos-server-1.1.4.zip速度太慢，码云上下载地址没有安装包。

采用从码云上下载源码，自行打包：https://gitee.com/mirrors/Nacos

下载完成之后，进入项目目录解压到本地  

在本文件夹 在dos窗口下利用命令：mvn -Prelease-nacos -DskipTests clean install -U  打包 ：

![](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1581396614727.png)

打包成功后的地址：

![img](https://img-blog.csdnimg.cn/20191228204813873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hsYTc3Nw==,size_16,color_FFFFFF,t_70)



### 3.启动服务器

#### （1）Linux/Unix/Mac

启动命令(standalone代表着单机模式运行，非集群模式):

```
sh startup.sh -m standalone
```

#### （2）Windows

启动命令：

```
cmd startup.cmd
```

或者双击startup.cmd运行文件。

![1581396861981](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1581396861981.png)

### 4 .测试服务器

服务启动后，访问地址：[http://ip](http://ip/):port/nacos

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zcHJpbmdjbG91ZC1vc3Mub3NzLWNuLXNoYW5naGFpLmFsaXl1bmNzLmNvbS9hbGliYWJhL2NoYXB0ZXIyL25hY29zNi5wbmc)

使用nacos/nacos登录

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zcHJpbmdjbG91ZC1vc3Mub3NzLWNuLXNoYW5naGFpLmFsaXl1bmNzLmNvbS9hbGliYWJhL2NoYXB0ZXIyL25hY29zNy5wbmc)

### 5 .关闭服务器

#### （1）Linux/Unix/Mac

```
sh shutdown.sh
```

#### （2）Windows

```
cmd shutdown.cmd
```

或者双击shutdown.cmd运行文件。

#### 6.修改Nacos Server默认端口

Nacos Server默认端口为8848，我们也可以进行修改：打开解压目录/conf/application.properties,我们可以对server.port进行修改后再启动Nacos Server：

```

# spring
 
server.contextPath=/nacos
server.servlet.contextPath=/nacos
server.port=8848
```

