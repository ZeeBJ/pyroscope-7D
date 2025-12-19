# Pyroscope 与 Grafana 集成使用指南

> **📌 重要提示：Grafana 界面更新说明**
> 
> 新版本的 Grafana（v10+）已经更新了界面结构：
> - **旧版本**：Configuration（配置）菜单
> - **新版本**：
>   - **Connections**（连接）- 用于管理数据源和插件
>   - **Administration**（管理）→ **Plugins and data**（插件和数据）- 用于安装和管理插件
> 
> 本指南已更新为适用于新版本的 Grafana 界面。如果您使用的是旧版本，菜单路径可能略有不同。

## 📖 目录
1. [Pyroscope 与 Grafana 的关系](#pyroscope-与-grafana-的关系)
2. [快速理解：架构概览](#快速理解架构概览)
3. [入门指南（中文翻译）](#入门指南中文翻译)
4. [部署指南（中文翻译）](#部署指南中文翻译)
5. [详细集成步骤](#详细集成步骤)

---

## Pyroscope 与 Grafana 的关系

### 核心概念

**Pyroscope** 和 **Grafana** 是两个互补的工具：

- **Pyroscope** = **数据收集和存储后端**
  - 负责收集应用程序的性能分析数据（CPU、内存等）
  - 存储和管理这些性能分析数据
  - 提供数据查询 API

- **Grafana** = **数据可视化前端**
  - 通过 **Explore Profiles UI** 插件连接到 Pyroscope
  - 提供美观的图形界面来查看和分析性能数据
  - 支持创建仪表盘、火焰图等可视化

### 工作流程

```
应用程序 → Pyroscope SDK/Agent → Pyroscope 服务器 → Grafana Explore Profiles UI → 用户查看
```

---

## 快速理解：架构概览

### 方式一：使用 Grafana Cloud（最简单）

```
1. 注册 Grafana Cloud 账号
2. Pyroscope 服务器和 Explore Profiles UI 已自动配置好
3. 只需在应用程序中集成 Pyroscope SDK
4. 数据自动显示在 Grafana 界面中
```

### 方式二：使用 Grafana OSS（自托管）

```
1. 安装并运行 Pyroscope 服务器（独立服务）
2. 在 Grafana 中安装 Explore Profiles UI 插件
3. 配置 Grafana 连接到 Pyroscope 服务器
4. 在应用程序中集成 Pyroscope SDK
5. 在 Grafana 界面中查看性能数据
```

---

## 入门指南（中文翻译）

### 什么是 Pyroscope？

Pyroscope 是一个**持续性能分析平台**，帮助您：
- 监控应用程序的 CPU、内存、I/O 使用情况
- 识别性能瓶颈
- 优化资源消耗

### 快速开始步骤

#### 步骤 1：启动 Pyroscope 服务器

**使用 Docker（推荐）：**
```bash
docker run -it -p 4040:4040 grafana/pyroscope
```

**使用 Homebrew（macOS）：**
```bash
brew install pyroscope-io/brew/pyroscope
brew services start pyroscope
```

启动后，Pyroscope 服务器会在 `http://localhost:4040` 运行。

#### 步骤 2：在应用程序中集成 Pyroscope SDK

根据您的编程语言，添加相应的 SDK。例如：

**Python 示例：**
```python
import pyroscope

pyroscope.configure(
    application_name="my.app",
    server_address="http://localhost:4040",
)
```

**Go 示例：**
```go
import "github.com/pyroscope-io/pyroscope/pkg/agent/profiler"

profiler.Start(profiler.Config{
    ApplicationName: "my.app",
    ServerAddress:   "http://localhost:4040",
})
```

#### 步骤 3：在 Grafana 中查看数据

**Grafana Cloud 用户：**
- 登录 Grafana Cloud
- Explore Profiles UI 已自动可用
- 直接开始查看数据

**Grafana OSS 用户：**
1. 安装 Explore Profiles 插件：
   - 在 Grafana 中，点击左侧菜单的 **Administration**（管理）
   - 展开后选择 **Plugins and data**（插件和数据）
   - 或者直接点击左侧菜单的 **Connections**（连接），然后选择 **Plugins** 标签页
   - 搜索 "Pyroscope" 或 "Explore Profiles"
   - 点击 **Install**（安装）
   - ✅ **验证安装**：如果看到 "Grafana Pyroscope" 插件显示为 **"Installed"**（已安装）状态，说明插件安装成功

2. 配置数据源（⚠️ **重要：这是必须的步骤**）：
   - 点击左侧菜单的 **Connections**（连接）
   - 选择 **Data sources**（数据源）
   - 点击 **Add new data source**（添加新数据源）
   - 搜索并选择 **Pyroscope**
   - 输入 Pyroscope 服务器地址：`http://localhost:4040`
   - 点击 **Save & Test**（保存并测试）

3. 查看数据：
   - 进入 **Explore** 菜单
   - 选择 **Explore Profiles** 应用
   - 开始探索您的性能数据

---

## 部署指南（中文翻译）

### Kubernetes 部署

#### 前置要求

- Kubernetes 集群（1.19+）
- kubectl 已配置
- 足够的存储空间（用于性能数据）

#### 部署步骤

**1. 创建命名空间**
```bash
kubectl create namespace pyroscope
```

**2. 部署 Pyroscope 服务器**

创建配置文件 `pyroscope-deployment.yaml`：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pyroscope
  namespace: pyroscope
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pyroscope
  template:
    metadata:
      labels:
        app: pyroscope
    spec:
      containers:
      - name: pyroscope
        image: grafana/pyroscope:latest
        ports:
        - containerPort: 4040
        env:
        - name: PYROSCOPE_STORAGE_PATH
          value: "/var/lib/pyroscope"
        volumeMounts:
        - name: storage
          mountPath: /var/lib/pyroscope
      volumes:
      - name: storage
        emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: pyroscope
  namespace: pyroscope
spec:
  selector:
    app: pyroscope
  ports:
  - port: 4040
    targetPort: 4040
  type: LoadBalancer
```

**3. 应用配置**
```bash
kubectl apply -f pyroscope-deployment.yaml
```

**4. 检查部署状态**
```bash
kubectl get pods -n pyroscope
kubectl get svc -n pyroscope
```

**5. 访问 Pyroscope**
```bash
# 获取服务地址
kubectl get svc pyroscope -n pyroscope

# 使用端口转发（如果使用 NodePort）
kubectl port-forward -n pyroscope svc/pyroscope 4040:4040
```

#### 在 Grafana 中配置

**1. 安装 Explore Profiles 插件**

如果使用 Helm 部署 Grafana：
```yaml
# values.yaml
plugins:
  - name: grafana-pyroscope-app
    version: latest
```

**2. 配置数据源**

在 Grafana 中：
- 点击左侧菜单的 **Connections**（连接）
- 选择 **Data sources**（数据源）
- 点击 **Add new data source**（添加新数据源）
- 搜索并选择 **Pyroscope**
- URL: `http://pyroscope.pyroscope.svc.cluster.local:4040`（Kubernetes 内部地址）
- 或使用外部地址：`http://<your-pyroscope-external-ip>:4040`
- 点击 **Save & Test**（保存并测试）

**3. 使用持久化存储（生产环境）**

修改部署配置，使用 PersistentVolume：

```yaml
volumeMounts:
- name: storage
  mountPath: /var/lib/pyroscope
volumes:
- name: storage
  persistentVolumeClaim:
    claimName: pyroscope-pvc
```

创建 PVC：
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pyroscope-pvc
  namespace: pyroscope
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 50Gi
```

---

## 详细集成步骤

### 场景 1：本地开发环境

#### 1. 启动 Pyroscope 服务器
```bash
docker run -d -p 4040:4040 --name pyroscope grafana/pyroscope
```

#### 2. 在代码中集成 SDK

**Python 应用示例：**
```python
# app.py
import pyroscope

# 配置 Pyroscope
pyroscope.configure(
    application_name="my-python-app",
    server_address="http://localhost:4040",
    tags={
        "environment": "development",
    }
)

# 您的应用代码
def main():
    # ... 您的业务逻辑
    pass

if __name__ == "__main__":
    main()
```

#### 3. 安装 Grafana（如果还没有）
```bash
# macOS
brew install grafana
brew services start grafana

# 或使用 Docker
docker run -d -p 3000:3000 --name grafana grafana/grafana
```

#### 4. 配置 Grafana

1. 访问 `http://localhost:3000`
2. 默认登录：`admin` / `admin`
3. 安装插件：
   - 点击左侧菜单的 **Administration**（管理）
   - 展开后选择 **Plugins and data**（插件和数据）
   - 或者点击左侧菜单的 **Connections**（连接），然后选择 **Plugins** 标签页
   - 搜索 "Pyroscope" 或 "Explore Profiles"
   - 点击 **Install**（安装）

4. 添加数据源：
   - 点击左侧菜单的 **Connections**（连接）
   - 选择 **Data sources**（数据源）
   - 点击 **Add new data source**（添加新数据源）
   - 搜索并选择 **Pyroscope**
   - URL: `http://host.docker.internal:4040`（如果 Grafana 在 Docker 中）
   - 或 `http://localhost:4040`（如果 Grafana 在本地）
   - 点击 **Save & Test**（保存并测试）

5. 查看数据：
   - 点击左侧菜单的 **Explore**
   - 选择 **Explore Profiles**
   - 选择您的应用名称
   - 查看火焰图和其他性能指标

### 场景 2：生产环境（Kubernetes）

#### 完整部署清单

**1. Pyroscope 服务器部署**
```yaml
# pyroscope-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: pyroscope-config
  namespace: pyroscope
data:
  config.yaml: |
    server:
      http-listen-address: 0.0.0.0:4040
    storage:
      path: /var/lib/pyroscope
```

**2. Grafana 配置（Helm）**
```yaml
# grafana-values.yaml
plugins:
  - name: grafana-pyroscope-app
    version: latest

datasources:
  datasources.yaml:
    apiVersion: 1
    datasources:
      - name: Pyroscope
        type: grafana-pyroscope-datasource
        url: http://pyroscope.pyroscope.svc.cluster.local:4040
        access: proxy
        isDefault: false
```

**3. 应用程序配置**

在您的应用部署配置中添加环境变量：
```yaml
env:
- name: PYROSCOPE_SERVER_ADDRESS
  value: "http://pyroscope.pyroscope.svc.cluster.local:4040"
- name: PYROSCOPE_APPLICATION_NAME
  value: "my-production-app"
```

---

## ✅ 安装状态检查清单

使用以下清单来确认您的 Pyroscope + Grafana 集成是否完整：

### 步骤 1：检查插件安装状态
- [ ] 在 Grafana 中，进入 **Connections** → **Plugins** 或 **Administration** → **Plugins and data**
- [ ] 搜索 "Pyroscope"
- [ ] 确认 "Grafana Pyroscope" 插件显示为 **"Installed"**（已安装）✅ **您已完成此步骤！**

### 步骤 2：检查 Pyroscope 服务器是否运行
- [ ] 确认 Pyroscope 服务器正在运行（使用 Docker 或 Homebrew 启动）
- [ ] 在浏览器中访问 `http://localhost:4040`，应该能看到 Pyroscope 的 Web 界面
- [ ] 如果无法访问，检查服务器是否正常启动

### 步骤 3：配置数据源（⚠️ 这是关键步骤）
- [ ] 在 Grafana 中，点击左侧菜单的 **Connections**（连接）
- [ ] 选择 **Data sources**（数据源）
- [ ] 点击 **Add new data source**（添加新数据源）
- [ ] 搜索并选择 **Pyroscope**
- [ ] 输入 Pyroscope 服务器地址：`http://localhost:4040`（或您的服务器地址）
- [ ] 点击 **Save & Test**（保存并测试）
- [ ] 确认显示 "Data source is working"（数据源工作正常）

### 步骤 4：检查应用程序是否在发送数据
- [ ] 确认您的应用程序已集成 Pyroscope SDK
- [ ] 确认应用程序配置了正确的 Pyroscope 服务器地址
- [ ] 运行应用程序一段时间，让数据开始收集

### 步骤 5：在 Grafana 中查看数据
- [ ] 点击左侧菜单的 **Explore**（探索）
- [ ] 在顶部选择器中选择 **Profiles**（性能分析）
- [ ] 应该能看到您的应用程序名称
- [ ] 选择应用程序后，应该能看到火焰图和其他性能数据

**如果所有步骤都完成，您的集成就成功了！** 🎉

---

## 常见问题解答

### Q1: Pyroscope 和 Grafana 必须一起使用吗？

**A:** 不一定。Pyroscope 服务器本身有一个简单的 Web UI（`http://localhost:4040`），但 Grafana 的 Explore Profiles UI 提供了更强大和美观的可视化体验。

### Q2: 我需要同时安装 Pyroscope 服务器和 Grafana 吗？

**A:** 是的。Pyroscope 服务器负责数据存储，Grafana 负责数据可视化。它们是两个独立的服务。

### Q3: Grafana Cloud 和 Grafana OSS 有什么区别？

**A:**
- **Grafana Cloud**: 托管服务，Pyroscope 服务器和 UI 都已配置好，开箱即用
- **Grafana OSS**: 需要自己部署和配置，但完全免费且可定制

### Q4: 插件显示 "Installed" 就说明可以使用了吗？

**A:** 不是的。插件安装只是第一步，您还需要：
1. ✅ 插件已安装（您已完成）
2. ⚠️ **配置数据源**：在 **Connections** → **Data sources** 中添加 Pyroscope 数据源
3. ⚠️ **确保 Pyroscope 服务器正在运行**
4. ⚠️ **确保应用程序正在发送数据到 Pyroscope 服务器**

只有完成所有步骤后，才能在 Grafana 中看到性能数据。

### Q5: 如何验证集成是否成功？

**A:** 
1. 检查 Pyroscope 服务器是否运行：访问 `http://localhost:4040`
2. 检查数据源配置：在 Grafana 的 **Connections** → **Data sources** 中，确认 Pyroscope 数据源显示为 "Working"
3. 检查应用程序是否在发送数据：查看 Pyroscope 服务器的日志
4. 在 Grafana 中查看：进入 **Explore** → **Profiles**，应该能看到您的应用名称和数据

### Q6: 数据如何从应用程序流向 Grafana？

**A:** 
```
应用程序代码 
  → Pyroscope SDK（收集性能数据）
    → Pyroscope 服务器（存储数据，端口 4040）
      → Grafana Explore Profiles UI（通过 API 查询数据并展示）
```

---

## 下一步

- 查看 [Pyroscope 架构文档](https://grafana.com/docs/pyroscope/latest/reference-pyroscope-architecture/)
- 学习如何为不同语言配置客户端：[客户端配置文档](https://grafana.com/docs/pyroscope/latest/configure-client/)
- 探索高级功能：多租户、数据保留策略等

---

## 相关链接

- [Pyroscope 官方文档](https://grafana.com/docs/pyroscope/latest/)
- [Explore Profiles GitHub](https://github.com/grafana/explore-profiles)
- [Grafana 插件目录](https://grafana.com/grafana/plugins/grafana-pyroscope-app/)

