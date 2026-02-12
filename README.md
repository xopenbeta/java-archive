# Java Archive

自动构建和打包多版本、多平台的 Java Development Kit (JDK)。

## 📦 支持的平台

### Java 版本与发行版

| Java 版本 | 发行版 | 说明 |
|-----------|--------|------|
| Java 8 | Azul Zulu | 对 Java 8 支持最佳，免费生产使用 |
| Java 11+ | Eclipse Temurin | 社区驱动，TCK 认证，推荐首选 |

> **关于 Java 8**: Eclipse Temurin 对 Java 8 的 API 支持有限制，我们使用 Azul Zulu 发行版来确保稳定性。Zulu 为 Java 8 提供免费的长期支持和安全更新（至少到 2030 年）。

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
git tag v202602121408
git push origin v202602121408
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
  - Windows aarch64 (仅 17, 21, 23)
```

**总计: 28 个构建任务** (Java 8-11: 4平台 × 2版本 + Java 17-23: 5平台 × 3版本)

#### 为什么 Java 8 使用不同的发行版？

Eclipse Temurin 对 Java 8 的 API 支持在 GitHub Actions 中存在限制，会导致版本解析错误。为确保稳定性和兼容性，我们为 Java 8 选用 **Azul Zulu**：

- ✅ **免费商用**: Zulu 对所有版本（包括 Java 8）提供免费使用
- ✅ **长期支持**: 提供安全更新至少到 2030 年
- ✅ **完整兼容**: 100% 通过 Java TCK 认证
- ✅ **广泛使用**: 被 Azure、Alpine Linux 等采用
