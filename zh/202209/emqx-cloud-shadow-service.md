全托管 [MQTT 消息云服务 EMQX Cloud](https://www.emqx.com/zh/cloud) 可以帮助用户轻松将各类物联网设备连接上云，提供与各类第三方服务的数据集成，助力用户进行高效的数据处理、存储与分析。

为了实现更加便捷的物联网数据处理，进一步简化用户构建物联网应用的开发流程，近日，EMQX Cloud 推出了一项新的增值服务——影子服务（Shadow Service ）。

## 功能详情

影子服务是 EMQX Cloud 提供的一个**设备数据缓存**服务，用户可以通过 Topic 以及 API 定义和使用，快速设计开发物联网应用。

此前，用户需要先通过 EMQX Cloud 的数据集成服务将物联网数据转存到第三方数据服务中，之后才能进行物联网数据处理分析和物联网应用的进一步开发工作。如：首先购买第三方数据集成资源云服务，再创建 VPC 对等连接打通 EMQX Cloud 和第三方云服务，最后创建开发物联网应用。

全新上线的影子服务所提供的设备数据缓存能力可以省去打通 EMQX Cloud 和第三方数据服务的步骤，用户可以实现在 EMQX Cloud 内部集中完成设备数据缓存、修改、查看，快速创建物模型、设备影子以及其他和数据上报及下发相关的应用，极大节省开发时间和成本。

![EMQX Cloud 影子服务](https://assets.emqx.com/images/6fb0b9da0b663623342234ebca50fe79.png)
 

## 应用场景

通过影子服务为用户提供的数据缓存能力，用户在无需配置外部存储和网络打通的情况下就可以实现很多应用的开发，非常适用于以下场景：

### 应用程序请求获取设备状态

- 设备网络不稳定，设备频繁上下线，无法正常响应应用程序的请求。
- 设备网络稳定，同时响应多个应用程序的请求，即使响应的结果一样，设备本身处理能力有限，也会无法负载多次请求。
- 设备传输信息，暂无数据消费者。应用程序上线后才需要查看最新设备信息。
- 设备传输信息，不同应用程序读取不同部分的信息。
- 设备传输多组信息，应用程序综合展现所有信息。

使用设备影子，设备状态变更只需同步状态给设备影子一次，应用程序请求获取设备状态，不论应用程序是否在线、请求数量多少、设备是否联网在线，都可从设备影子中获取设备当前状态，实现应用程序与设备解耦。

### 应用程序下发指令给设备，变更设备状态

- 设备处于下线状态，或设备网络不稳定，设备频繁上下线，应用程序发送控制指令给设备，设备不在线，指令就会发送失败。

使用设备影子机制，可以将应用程序下发的指令，携带时间戳存储到设备影子中。设备再上线时，获取设备影子中指令，并根据时间戳确定是否执行。

设备影子场景模拟实现可参考：[https://docs.emqx.com/zh/cloud/latest/shadow_service/device_shadow.html](https://docs.emqx.com/zh/cloud/latest/shadow_service/device_shadow.html) 

## 使用指南

### 开通与计费说明

目前影子服务提供 1G 规格 7 天的免费试用。您可以登陆 EMQX Cloud，通过顶部菜单「增值服务」模块或左侧菜单「影子服务」模块开通影子服务。

![开通影子服务](https://assets.emqx.com/images/862450b71ca57fd86be6485d975e1439.png)

顶部菜单「增值服务」-> 「影子服务」-> 开通服务

![开通影子服务](https://assets.emqx.com/images/3ed1e2a68cfd89ffea18eb5e80458407.png)

左侧菜单「影子服务」-> 开通服务

> 注：由于影子服务使用到阿里云云计算资源，目前影子服务仅限**阿里云的专业版**部署可以使用。由于地域限制，如果您的部署在**阿里云张家口**地区，也无法开通本服务。

影子服务的费用由存储空间费用、调用次数费用、出网流量费三个部分构成。

您可以根据预估所需要的影子模型数量选择不同存储空间规格，不同存储规格的价格和与顾客创建影子模型数量可参考如下表格

| **存储规格** | **存储空间计价** | **预估可创建影子模型数量** | **调用次数费用**                       | **出网流量费**                                               |
| :----------- | :--------------- | :------------------------- | :------------------------------------- | :----------------------------------------------------------- |
| 1G           | 0.5元/小时       | 7000个                     | 0.03 元/万次，未满 1 万次按 1 万次计费 | 出网流量费用与当前部署流量共同计算，优先消费每月免费流量，超出部分按照 1.5 元/GB 计算。 |
| 2G           | 1元/小时         | 14000个                    |                                        |                                                              |
| 4G           | 2元/小时         | 28000个                    |                                        |                                                              |

![影子服务收费标准](https://assets.emqx.com/images/3f38744884d3532a06d8a5a050864196.png)

具体计费标准和计费示例可查看：[https://docs.emqx.com/zh/cloud/latest/shadow_service/pricing.html](https://docs.emqx.com/zh/cloud/latest/shadow_service/pricing.html)

### 功能页面导览

开通服务后，您可以在导航栏找到「使用统计」、「影子模型列表」和「API」三个页面。

在「使用统计」页面，您可以通过存储使用量、本月调用次数和不同时间维度使用量变化折线图及时了解当前部署的影子服务使用量，对业务用量进行监控和预警。

![使用统计](https://assets.emqx.com/images/4857250ac1d1adad06b7427293b9989c.png)

> 注：服务系统默认将占用约 90MB 的存储空间。

在「API」页面，您可以了解到关于创建、查询、更新、删除影子模型（信息）的 API 定义说明。您的物联网业务可以通过这些 API 来获取影子服务的相关信息，加速物联网应用开发。

![影子服务 API](https://assets.emqx.com/images/4251b9976e4114113abb070d5e9c8445.png)

同时，我们为您提供了多个调用示例供参考：[https://docs.emqx.com/zh/cloud/latest/shadow_service/invoke.html](https://docs.emqx.com/zh/cloud/latest/shadow_service/invoke.html)

在「影子模型列表」页面，您可以添加、编辑、修改影子模型，并且可以通过模板批量导入自定义的影子模型。

![影子模型列表](https://assets.emqx.com/images/19825febb5c8092e783851275894116d.png)

### 功能使用说明

点击「添加」，填写相关信息，点击「确认」即可创建影子模型。

![添加影子模型](https://assets.emqx.com/images/e332cd1db9977fbd6664204b50d7d278.png)

影子模型字段说明：

| **字段信息** | **是否必填** | **填写说明**                                                 |
| ------------ | ------------ | ------------------------------------------------------------ |
| 名称         | 是           | 只允许 3 到 50 个字符，并且只能包含中文字符、字母、数字、"-"、"_", "." |
| ID           | 是           | ID是该影子模型的全局唯一标识，将用到 Topic 和 API中，如不填写系统将自动生成 |
| 备注         | 否           | 无特别说明                                                   |
| 发布主题     | 系统填写     | 根据 ID 系统自动生成，用于设备端消息的发布                   |
| 订阅主题     | 系统填写     | 根据 ID 系统自动生成，用于设备端消息的接收                   |

点击影子模型列表中的 ID，进入影子模型详情页面。在此页面可以查看和修改当前模型的名称、备注。同时可以看到模型 JSON 最新的数据。并且可以对 JSON 进行修改。

![影子模型 JSON](https://assets.emqx.com/images/3183c9b5096ce914d060cf99d08a5b4b.png)
 
借助开箱即用的影子服务，各个行业不同业务场景下的数据缓存需求都可以得到满足。影子服务提供的 MQTT 设备接入与消息缓存一体化能力，将为加速物联网平台与应用开发提供动力。


<section class="promotion">
    <div>
        免费试用 EMQX Cloud
        <div class="is-size-14 is-text-normal has-text-weight-normal">无须绑定信用卡</div>
    </div>
    <a href="https://www.emqx.com/zh/signup?continue=https://cloud.emqx.com/console/deployments/0?oper=new" class="button is-gradient px-5">开始试用 →</a>
</section>
