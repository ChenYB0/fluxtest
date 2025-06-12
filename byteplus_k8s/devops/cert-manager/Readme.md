# cert-manager & reflector 部署（基于 Kustomize + Helm）

本目录用于通过 Kustomize 结合 Helm Charts 部署 cert-manager 及 reflector 到 Kubernetes 集群，适用于自动化证书管理和证书同步场景。

## 目录结构

- `kustomization.yaml`：Kustomize 主配置文件，集成 Helm Charts。
- `namespace.yaml`：定义 cert-manager 专用命名空间（需自行补充或已存在）。
- `values.yaml`：cert-manager 的自定义 values 配置（可选，根据实际需求调整）。

## 主要配置说明

- **namespace**:  
  所有资源部署到 `cert-manager` 命名空间，建议独立管理。

- **helmCharts**:  
  - `cert-manager`  
    - 来源：[Jetstack 官方 Helm 仓库](https://charts.jetstack.io)
    - 版本：v1.17.2（可根据需要修改）
    - 启用 CRDs（CustomResourceDefinitions），自动安装相关自定义资源。
    - 支持自定义 values（可通过 `values.yaml` 或 `valuesInline` 配置）。
  - `reflector`  
    - 来源：[EmberStack Helm 仓库](https://emberstack.github.io/helm-charts)
    - 版本：9.1.11
    - 用于自动同步 Secret/ConfigMap 跨 namespace。

- **resources**:  
  - `namespace.yaml`：确保 `cert-manager` 命名空间存在。

## 使用方法

1. **准备环境**
   - [kubectl](https://kubernetes.io/docs/tasks/tools/)
   - [kustomize](https://kubectl.docs.kubernetes.io/installation/kustomize/)。
   - Kubernetes 集群已连接。

2. **应用 Kustomize 配置**

   ```sh
   # 切换到本目录
   cd byteplus_k8s/devops/cert-manager

   # 应用配置
   kustomize build --enable-helm | kubectl apply -
   ```

3. **验证部署**

   ```sh
   kubectl get pods -n cert-manager
   kubectl get crds | grep cert-manager
   ```

4. **自定义配置**
   - 如需调整 cert-manager 配置，可编辑 `values.yaml` 或在 `kustomization.yaml` 的 `valuesInline` 字段中修改。

## 参考链接

- [cert-manager 官方文档](https://cert-manager.io/docs/)
- [reflector 项目地址](https://github.com/emberstack/kubernetes-reflector)
- [Kustomize 官方文档](https://kubectl.docs.kubernetes.io/pages/app_customization/introduction.html)

---

