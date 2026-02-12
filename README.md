# Java Archive

自动构建和打包多版本、多平台的 Java Development Kit (JDK)。

## 📦 支持的平台

### Java 版本与发行版

| Java 版本 | 发行版 | 说明 |
|-----------|--------|------|
| Java 8-23 | Eclipse Temurin | 社区驱动，TCK 认证，统一使用 |

> **版本策略**: 
> - 使用主版本号（如 '8', '11', '17', '21'），自动获取最新的安全补丁版本
> - 确保始终使用最新的稳定和安全更新
> - Eclipse Temurin 为所有支持的版本提供一致的体验

### macOS (全部在 Apple Silicon 上编译)
- **x64** (适用于 Intel 芯片 Mac) - 在 macOS 14 上交叉编译
- **aarch64** (Apple Silicon M1/M2/M3/M4 原生支持) - 在 macOS 14 上编译

> **注意**: GitHub Actions 不再提供 Intel Mac runners，所有 macOS 构建现在都在 Apple Silicon 上进行。x64 版本通过交叉编译生成，完全支持 Intel Mac。

### Linux
- **x64** (AMD64)

### Windows
- **x64** (64位)
- **aarch64** (ARM 64位) - 仅 Java 17, 21, 23

## 🤖 自动构建

### 触发条件

构建会在以下情况下自动触发：
**使用标签触发构建**:
```bash
git tag v202602121418
git push origin v202602121418
```

### 构建矩阵

工作流使用矩阵策略同时构建多个组合：

```yaml
java-versions: ['8', '11', '17', '21', '23']
platforms:
  - macOS x64 (在 Apple Silicon 上交叉编译)
  - macOS aarch64 (原生编译)
  - Linux x64
  - Windows x64
  - Windows aarch64
```
