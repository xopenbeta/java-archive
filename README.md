# Java Archive

自动构建和打包多版本、多平台的 Java Development Kit (JDK)。

## 🚀 特性

- ✅ **多版本支持**: Java 8, 11, 17, 21, 23
- ✅ **跨平台**: macOS, Linux, Windows
- ✅ **多架构**: x86, x64, arm64
- ✅ **自动化**: GitHub Actions 自动构建
- ✅ **安全性**: 包含 SHA256 校验和

## 📦 支持的平台

### macOS
- **x64** (Intel 芯片) - 使用 macOS 13
- **aarch64** (Apple Silicon M1/M2/M3/M4) - 使用 macOS 14

### Linux
- **x64** (AMD64)
- **aarch64** (ARM 64位)

### Windows
- **x64** (64位)
- **aarch64** (ARM 64位)

## 🔧 使用方法

### 1. 下载预构建的包

从 [Releases](../../releases) 页面下载适合你平台的 JDK 压缩包。

### 2. 解压

**Unix/Linux/macOS:**
```bash
tar -xzf jdk-<version>-<os>-<arch>.tar.gz
```

**Windows:**
```powershell
Expand-Archive jdk-<version>-windows-<arch>.zip -DestinationPath C:\Java\jdk-<version>
```

### 3. 配置环境变量

**Unix/Linux/macOS (bash/zsh):**
```bash
export JAVA_HOME=/path/to/extracted/jdk
export PATH=$JAVA_HOME/bin:$PATH
```

**Windows (命令提示符):**
```cmd
set JAVA_HOME=C:\path\to\extracted\jdk
set PATH=%JAVA_HOME%\bin;%PATH%
```

**Windows (PowerShell):**
```powershell
$env:JAVA_HOME = "C:\path\to\extracted\jdk"
$env:PATH = "$env:JAVA_HOME\bin;$env:PATH"
```

### 4. 验证安装

```bash
java -version
```

## 🔐 校验下载

每个归档文件都附带一个 SHA256 校验和文件。

**Unix/Linux/macOS:**
```bash
sha256sum -c jdk-<version>-<os>-<arch>.tar.gz.sha256
```

**Windows (PowerShell):**
```powershell
$expectedHash = Get-Content jdk-<version>-windows-<arch>.zip.sha256 | Select-Object -First 1 | ForEach-Object { $_.Split()[0] }
$actualHash = (Get-FileHash jdk-<version>-windows-<arch>.zip -Algorithm SHA256).Hash
if ($expectedHash -eq $actualHash.ToLower()) { 
    Write-Host "✓ 校验通过" -ForegroundColor Green 
} else { 
    Write-Host "✗ 校验失败" -ForegroundColor Red 
}
```

## 🤖 自动构建

### 触发条件

构建会在以下情况下自动触发：

1. **Push** 到 `main` 或 `master` 分支
2. **Pull Request** 到 `main` 或 `master` 分支
3. **手动触发** (在 Actions 页面)
4. **定时任务** (每周日自动运行)

### 构建矩阵

工作流使用矩阵策略同时构建多个组合：

```yaml
java-version: ['8', '11', '17', '21', '23']
platforms:
  - macOS x64
  - macOS aarch64
  - Linux x64
  - Linux aarch64
  - Windows x64
  - Windows aarch64
```

总计: **30 个构建任务** (5 个版本 × 6 个平台)

## 📝 自定义版本

如需添加或修改 Java 版本，编辑 [.github/workflows/build-java.yml](.github/workflows/build-java.yml):

```yaml
matrix:
  java-version: ['8', '11', '17', '21', '23', '24']  # 添加新版本
```

## 📄 许可证

本项目仅用于自动打包 JDK。每个 JDK 的许可证遵循其原始发行版的许可证。

- [Eclipse Temurin](https://adoptium.net/) (使用的发行版)

## 🤝 贡献

欢迎提交 Issues 和 Pull Requests！

## 📚 参考资源

- [详细配置文档](docs/CONFIGURATION.md)
- [架构支持说明](docs/ARCHITECTURE_SUPPORT.md)
- [actions/setup-java](https://github.com/actions/setup-java)
- [Eclipse Temurin](https://adoptium.net/)
- [GitHub Actions 文档](https://docs.github.com/en/actions)

## ⚠️ 注意事项

1. **存储空间**: 每个构建会生成较大的归档文件，注意 GitHub 仓库的存储限制
2. **构建时间**: 完整的矩阵构建可能需要较长时间
3. **Runner 限制**: 某些架构（如 arm64）的 Runner 可能有使用限制
4. **定期清理**: Artifacts 保留 90 天，定期清理旧的 Releases

## 🔍 故障排查

### Linux aarch64 构建失败

如果 `ubuntu-22.04-arm64` runner 不可用，可以使用 QEMU 替代：

```yaml
- name: Set up QEMU
  uses: docker/setup-qemu-action@v3
  
- name: Build on arm64
  uses: uraimo/run-on-arch-action@v2
  with:
    arch: aarch64
    distro: ubuntu22.04
```

### 特定版本不支持某些架构

部分 Java 版本可能不支持某些架构，可以在矩阵中添加排除规则：

```yaml
matrix:
  exclude:
    # 例如：Java 8 可能不支持 Windows ARM64
    - java-version: '8'
      os: windows-2022
      arch: aarch64
```

---

**生成时间**: $(date)  
**仓库**: [java-archive](https://github.com/your-username/java-archive)
