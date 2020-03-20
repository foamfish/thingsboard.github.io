---
layout: docwithnav
title: 源代码安装网关

---

### 源代码安装

从源代码安装ThingsBoard网关您应遵循以下步骤：
  
**1. 使用apt将所需的库安装到系统中：**
```bash
sudo apt install python3-dev python3-pip libglib2.0-dev git 
```
{: .copy-code}

**2. 从Github克隆源代码：**
```bash
git clone https://github.com/thingsboard/thingsboard-gateway.git
```
{: .copy-code}

**3. 进入代码目录：**
```bash
cd thingsboard-gateway
```
{: .copy-code}

**4. 使用setup.py脚本安装python模块：**
```bash
python3 setup.py install
```
{: .copy-code}

**5. 创建“日志”文件夹：**
```bash
mkdir logs
```
{: .copy-code}

**6. 将网关配置为与您的ThingsBoard平台实例一起使用,[本指南](/docs/iot-gateway/all_configuration/)** *或者仅运行以测试安装结果，例如在下一步中*
   
**7. 运行网关，检查安装结果：**
```bash
python3 ./thingsboard_gateway/tb_gateway.py
```
{: .copy-code}
