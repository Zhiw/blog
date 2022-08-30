近年来，汽车产业向「电气化、智能化、网联化、共享化」快速演进，「软件定义汽车」模式和 SOA 理念在汽车研发和设计领域逐渐深入。无论是作为智能网联汽车云端底座的 TSP 平台、基于单车智能 ADAS 的自动驾驶体系，还是实现软件定义汽车的 SOA 框架，均需要更加灵活的软件开发、迭代、复用和运行架构保障。

云原生技术的快速发展和落地，大大改变了车联网应用传统的开发和运行方式。以其灵活、弹性、敏捷、自动化、高可用、易扩展等特性，为汽车行业智能网联和自动驾驶相关软件的开发和运行，提供了平台层面的助力，解决了车联网平台在新趋势下面临的上述挑战。

>**云原生：** 在 CNCF（云原生计算基金会）的定义中，云原生技术有利于各组织在公有云、私有云和混合云等新型动态环境中，构建和运行可弹性扩展的应用。云原生的代表技术包括容器、服务网格、微服务、不可变基础设施和声明式 API。

本文旨在深入分析云原生技术如何作用于车联网物联网基础设施构建，基于体系中最关键的车端消息采集、移动、处理和分析领域，结合 EMQ 相关数据基础设施软件，实现云原生的车联网基础设施架构。

## 传统车联网平台构建的挑战

传统车联网的消息处理框架在构建底层资源和运行平台端的整体框架时，往往采用本地数据中心虚拟机/物理机或云服务商虚拟机进行部署。此种模式在联网车辆快速增多、车端上传数据愈加复杂的场景下，通常会面临如下的痛点和挑战：

1. 各大主机厂若想将在私有数据中心提供车联网服务的模式转变为通过公有云提供车联网服务，传统的以虚拟机为应用承载方式的架构会过于沉重，无法平滑实现车联网混合云迁移需求和混合云的部署模式；
2. 随着智能化和网联化的快速发展，车联网系统对于消息处理平台的软件迭代能力要求逐渐提高，传统的软件迭代模式无法应对智能网联对于车联网系统敏捷、灵活和快速的能力要求，针对纷繁复杂的消息处理需求和软件迭代也无法响应；
3. 车联网系统作为主机厂同终端客户最重要的实时沟通系统，需要具备极高的可靠性、可用性和可支撑性。消息处理平台作为核心应用组件，应具备弹性的资源获取能力和自动化伸缩、运维等运营支撑能力。传统的巨石型应用架构和虚拟机部署模式无法满足消息处理平台弹性和自复位的能力要求。

## 云原生技术赋能新一代车联网消息处理

CNCF（Cloud Native Computing Foundation）旗下项目中？以容器编排系统 Kubernetes 最为核心和基础。Kubernetes 通过将应用程序的容器组合成逻辑单元，以便于管理与服务发现，其为开源系统，可以自由地部署在企业内部、私有云、混合云或公有云，方便用户做出自由选择。

![Kubernetes](https://assets.emqx.com/images/0d5e91a11f099f3d034f387c4392795c.jpeg)

越来越多的主机厂在业务平台的生产交付场景中，采用云原生技术打造以下能力，助力智能网联汽车的应用演进和发展。

- **统一部署**：提供屏蔽底层资源型基础设施差异，一次构建，多处使用的能力
- **易于实施**：提供 CaaI(Config as an Infrastructure)，即配置即设施的能力，达到配置即运行时效果的能力
- **弹性扩容**：根据业务使用情况进行资源型资源的快速弹性伸缩，提供运行时伸缩，业务应用无感知的能力
- **监控告警**：提供完善的监控告警体系，满足生产环境后期维护的可控性能力
- **版本迭代可控**：提供风险可控的版本变更手段，包括版本追溯与回滚的能力

## 基于 Operator 的 EMQX 云原生框架

早期 EMQ 产品云原生部署采用的是 Helm 部署方式，Operator 模式的出现为实现自定义资源提供了标准的解决方案，解决了通用 Kubernetes 基础模型元素无法支撑不同业务领域下复杂自动化场景的痛点，为实现更加简单、高效的 EMQX 部署提供了全新的方式。

简单来说，Operator 模式是一组自定义控制器的集合以及由这些控制器所管理的一系列自定义资源，我们将不再关注于 Pod（容器）、ConfigMap 等基础模型元素，而是将它们聚合为一个应用或者服务。Operator 通过控制器的协调循环来使自定义应用达到我们期望的状态，我们只需要关注该应用的期望状态，通过自定义控制器协调循环逻辑来达到 7*24 小时不间断的应用或者服务的声明周期管理。基于 Operator 的 EMQX 云原生框架，使得用户可以轻松基于 Kubernetes 的模式部署和运维 EMQX 集群。

### Operator VS Helm

Operator 的管理不仅限于 Pod，也可以是多个资源（比如 SVC 域名等）。从这个角度上来说，Operator 跟 Helm 一样，也是具有编排能力的。从编排角度来看，Helm 与 Operator 有非常多的共性，很难对两者的作用进行区分。Helm 也可以完成分布式系统的部署。

那么 Operator 跟 Helm 又有什么样的区别呢？

- Helm 的侧重点在于多种多个的资源管理，而对生命周期的管理主要包括创建更新和删除，Helm 通过命令驱动整个的生命周期。
- Operator 对于资源的管理则不仅是创建和交付。由于其可以通过 watch 的方式获取相关资源的变化事件，因此可以实现高可用、可扩展、故障恢复等运维操作。因此 Operator 对于生命周期的管理包括创建、故障恢复、高可用、升级、扩容缩容、异常处理以及最终的清理等等。
- 如果把 Helm 比作 RPM，那么 Operator 就是 systemd。RPM 负责应用的安装、删除，而 systemd 则负责应用的启动、重启等等操作。

### Operator 工作原理

Operator 使用自定义资源（CR）管理应用及其组件的自定义 Kubernetes 控制器，自定义资源 Kubernetes 中 API 扩展，自定义资源配置 CRD 会明确 CR 并列出 Operator 用户可用的所有配置，Operator 监视 CR 类型并且采取特定于应用的操作，确保当前状态与该资源的理想状态相符。

![Operator 工作原理](https://assets.emqx.com/images/9f9901b43ae34d354a72742efeb03fe4.png)


Operator 中主要有以下几种对象：

- CRD：自定义资源的定义，Kubernetes API 服务器会为你所指定的每一个 CRD 版本生成 RESTful 的资源路径。一个 CRD 其实就是定义自己应用业务模型的地方，可以根据业务的需求，完全定制自己所需要的资源对象，如 EMQX Broker、EMQX Enterprise 等这些都是可以被 Kubernetes 直接操作和管理的自定义的资源对象。
- CR：自定义资源，即 CRD 的一个具体实例，是具体的可管理的 Kubernetes 资源对象，可以对其进行正常的生命周期管理，如创建、删除、修改、查询等，同时 CR 对象一般还会包含运行时的状态，如当前的 CR 的真实的状态是什么，用于观察和判断，CR 对象的真正所处于的状态。
- Controller：其实就是控制器真正的用武之地了，它会循环处理工作队列中的动作，按照逻辑协调应用当前状态至期望状态。如观察一个CR 对象被创建了之后，会根据实现的逻辑来处理 CR，让 CR 对象的状态以及 CR 对象所负责的业务逻辑慢慢向期望的状态上靠近，最终达到期望的效果。举例来说：如果定义了一个 EMQX Broker 的 Operator，那在创建 EMQX Broker 的时候，就会一直协调和观察 EMQX Broker 真正的集群是否已创建好，以及每个节点的状态和可用性是否健康。一旦发现不符合期望的状态就会继续协调，始终保持基于事件的机制不断检查和协调，以保证期望的状态。

## EMQX 在车联网场景中的云原生实践

基于 Operator 模式，我们提供了 [EMQX Operator](https://www.emqx.com/zh/emqx-kubernetes-operator) 来帮助客户在 Kubernetes 的环境上快速构建和管理 EMQX 集群。

EMQX Operator 是一个用来帮助用户在 Kubernetes 的环境上快速创建和管理 EMQX 集群的工具。 它可以大大简化部署和管理 EMQX 集群的流程，将其变成一种低成本的、标准化的、可重复性的能力。

![EMQX 在车联网场景中的云原生实践](https://assets.emqx.com/images/fe40aadc70e7c9a875f72a5668b6b32e.png)

作为车联网的核心底层支撑组件，EMQX 可以通过 Kubernetes Operator 进行部署、管理和运维。通过基于云原生的消息处理平台，为车联网场景中的客户开发和运维部署带来了诸多好处：

- 无感和滚动更新：以云原生技术构建的车联网消息处理框架，可以轻松实现车联网应用的灰度发布，使得车联网系统升级迭代过程中无需中断服务，让车联网应用的使用者完全无感知，实现无感迭代和滚动更新，提升用户体验；
- 统一监控：基于云原生技术，车联网系统的运维者可轻松进行应用集群和节点的监控和管理。通过与 Prometheus 等监控工具的集成，可以轻松获取车联网中最重要的消息处理平台的运行信息，从而更直观清晰地了解业务运行情况，进行相应的业务迭代；
- 快速部署：云原生技术可以让车联网开发和运维人员快速部署和调整应用，无论是基于公有云还是私有云环境，均可以轻松部署相应的 EMQX 集群，实现对业务的支撑；
- 标准化镜像：EMQX 基于云原生环境提供了标准化的官方容器镜像，当用户系统通过镜像来进行 EMQX 的部署和构建时，可基于标准化镜像进行相应的工作，对于车联网环境中的快速发布和多次构建等需求提供了很好的支撑；
- 弹性伸缩：随着车联网应用的深入，整个消息处理框架所需要对接的应用逐步扩展和车联网规模的增大，消息处理平台对资源的弹性能力要求也越来越高，通过 Kubernetes 弹性灵活的资源支撑模式，可以针对应用的使用量进行资源获取、增加和释放，从而节省资源，降低运营成本；
- 快速迭代：基于云原生框架构建的车联网应用，可利用持续集成和持续交付流水线实现应用的即时更新和发布，支撑业务对于车联网快速迭代的需求；

## 结语

随着云原生理念在各行业的深入，我们相信云原生也将为车联网领域的平台构建与应用开发模式注入新的动力。未来 EMQ 将围绕对 EMQX 新版本特性的支持，不断完善迭代 EMQX Operator，致力于在云原生模式下提供更加丰富可靠的数据基础设施能力，服务于车联网行业。



<section class="promotion">
    <div>
        免费试用 EMQX Cloud
        <div class="is-size-14 is-text-normal has-text-weight-normal">全托管的云原生 MQTT 消息服务</div>
    </div>
    <a href="https://www.emqx.com/zh/signup?continue=https://cloud.emqx.com/console/deployments/0?oper=new" class="button is-gradient px-5">开始试用 →</a>
</section>



## 本系列中的其它文章

- [车联网平台搭建从入门到精通 01 | 车联网场景中的 MQTT 协议](https://www.emqx.com/zh/blog/mqtt-for-internet-of-vehicles)

- [车联网平台搭建从入门到精通 02 | 千万级车联网 MQTT 消息平台架构设计](https://www.emqx.com/zh/blog/mqtt-messaging-platform-for-internet-of-vehicles)
- [车联网平台搭建从入门到精通 03 | 车联网 TSP 平台场景中的 MQTT 主题设计](https://www.emqx.com/zh/blog/mqtt-topic-design-for-internet-of-vehicles)
- [车联网平台搭建从入门到精通 04 | MQTT QoS 设计：车联网平台消息传输质量保障](https://www.emqx.com/zh/blog/mqtt-qos-design-for-internet-of-vehicles)
- [车联网平台搭建从入门到精通 05 | 车联网平台百万级消息吞吐架构设计](https://www.emqx.com/zh/blog/million-level-message-throughput-architecture-design-for-internet-of-vehicles)
- [车联网平台搭建从入门到精通 06 | 车联网通信安全之 SSL/TLS 协议](https://www.emqx.com/zh/blog/ssl-tls-for-internet-of-vehicles-communication-security)
- [车联网平台搭建从入门到精通 07 | 国密在车联网安全认证场景中的应用](https://www.emqx.com/zh/blog/application-of-gmsm-in-internet-of-vehicles-security-authentication-scenario)