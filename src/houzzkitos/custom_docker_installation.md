---
title: 应用自定义安装
index: true
order: 6
author: Yin Baiyuan
date: 2024-4-24
icon: book
category:
  - 开发指南
---

## 入口

在`应用商店`点击`自定义安装`按钮可以安装任意docker应用。

## 表单填写规范

### Docker镜像名称  
`必填项` `文本框` 

- 填写 *Docker Hub* 中存在的镜像名称。例如 `portainer/portainer`

### Docker镜像版本  
`必填项` `下拉列表`
* Docker镜像名称填写正确后，此处将自动从docker hub获取镜像的版本列表。如果显示`暂无数据`，请检查镜像名称是否填写错误。
::: tip 注意
版本列表只显示系统架构为`linux/arm64`的TAG
:::

### 应用名称  
`必填项` `文本框` 
* 显示在主页面图标下面
* 长度255个字符以内，支持大小写字母、数字、中文和@ : . ?[]{}

### 容器主机名  
`必填项` `文本框` 
* 定义容器主机的hostname，参数会直接写入容器主机的/etc/hostname文件中

### 应用图标  
`必填项` `文件选择`
* 定义应用在主页面显示的图标
* 图标上传后，会被裁剪成96*96像素大小的图片，然后保存

### Web UI
`非必填项` `文本框`
* 填入一个1024-65565范围内的端口号。当在主页面点击图标时，网页将打开一个当前域名并拼接此端口号的新页面。例如 `192.168.1.100:9000`
* 如果不填，在主页面点击图标时，网页不跳转

### 网络
`必填项` `下拉列表`
* 用于指定docker容器的网络模式
  * `bridge`模式下，Docker会为容器创建一个虚拟网络接口，并分配一个独立IP地址。容器之间相互隔离，只能在桥接网络中通信
  * `host`模式下，容器直接使用HOUZZkit主机的网络资源。这样，容器中的应用程序可以直接访问HOUZZkit所在网络上的服务
* 默认选择`bridge`模式

### 端口
`非必填项` `文本框`
* 用于指定HOUZZkit主机和容器之间的端口映射关系
* 端口映射是通过HOUZZkit主机端口和容器端口建立通信实现的，通信协议可选择`TCP`或`UDP`，默认为`TCP`
* 端口映射可以添加多个或不添加
* 不论应用内还是应用间，主机端口不可重复

### 卷
`非必填项` `文本框` 
* 用于指定HOUZZkit主机和容器内文件或文件夹的映射关系
* 当容器在指定的容器路径创建或修改文件后，HOUZZkit主机就可以在设定的主机路径中直接访问。由此产生的文件，不会因为容器的重启或删除而丢失
* 卷可以添加多个或不添加

### 环境变量
`非必填项` `文本框` 
* 用于指定在容器生命周期内生效的环境变量，指定一个环境变量需要同时定义键值对
* 环境变量可以添加多个或不添加

### 设备
`非必填项` `文本框` 
* 用于将HOUZZkit主机硬件设备映射到容器，以便让容器直接访问
* 设备可以添加多个或不添加

### 容器命令
`非必填项` `文本框` 
* 用于定义容器启动时，需要执行的默认命令。如果在运行容器时提供了命令行参数，此处定义中命令将被覆盖。主要用于执行容器内的SHELL脚本
* 命令可以添加多个或不添加。添加多个时，只有最后一个会生效

### 入口点
`非必填项` `文本框` 
* 用于指定容器启动时的固定执行命令，该命令不会被覆盖。主要用于启动容器内的命令行解释器
* 默认值为 `/bin/bash`，一般不用修改。如果右击应用图标，点击`终端`，无法进入容器终端时，可以尝试修改此值

### 使用高权限执行容器
`非必填项`  `开关按钮`
* 开启后，容器以`特权模式`运行。Docker会赋予容器**几乎**与主机相同的权限
* 如非必要，请勿开启

### 重启策略
`非必填项`  `下拉列表`
* 用于指定当容器退出时，守护进程以何种策略重启容器
* 可以选择的重启策略如下
  * `no` 默认策略，在容器退出时不重启容器
  * `always` 在容器退出时总是重启容器
  * `unless-stopped` 在容器退出时总是重启容器，当HOUZZkit主机重启时，在重启前已经停止的容器不会被重启
  * `on-failure` 在容器非正常退出时（退出状态非0），才会重启容器

### 内存限制
`非必填项` `文本框` 
* 用于指定容器可以使用的最大内存，单位为`MB`
* 设置为`0`MB，则不限制

### CPU限制
`非必填项`  `下拉列表`
* 用于指定容器可以利用的CPU最大使用率
* 有 `无` `低` `中` `高` 四种选项
  * `无` -不限制
  * `低` -25%
  * `中` -50%
  * `高` -75%

### 应用初始化配置文件
`非必填项`  `文件选择`
* 用于定义应用正常工作前需要完成的一系列配置。正确的定义后，应用安装完后容器将不会自动，直到用户完成`启动设置`
* 配置文件为yaml格式的文本文件，具体配置方法，请参考 [开发者指南](../developers/houzzkit_docker_package_make.md)

### 应用卸载脚本
`非必填项`  `文件选择`
* 用于定义应用卸载时，除删除容器以外，还需要进行的一系列卸载操作
* 卸载脚本为sh格式的自动化脚本，具体配置方法，请参考 [开发者指南](../developers/houzzkit_docker_package_make.md)

### 资源文件
`非必填项`  `文件选择`
* 用于定义应用初始化配置过程中，需要用到的一系列资源文件
* 上传资源可以为文件或者文件夹，具体使用方法，请参考 [开发者指南](../developers/houzzkit_docker_package_make.md)