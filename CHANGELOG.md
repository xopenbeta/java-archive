# 更新日志

## 2026-02-12 - v1.6 暂时禁用 Windows ARM64

### ⚠️ Windows ARM64 构建已暂时注释

**原因**: GitHub Actions 目前没有提供公共的 `windows-11-arm64` runner，导致构建任务无限等待。

**影响**:
- Windows ARM64: 所有版本 (Java 8, 11, 17, 21, 23) 暂时不可用
- 总构建任务: 从 33 个减少到 28 个

**当前可用平台**:
- ✅ macOS x64 (5 个版本)
- ✅ macOS aarch64 (5 个版本)
- ✅ Linux x64 (5 个版本)
- ✅ Windows x64 (5 个版本)
- ❌ Windows aarch64 (暂不可用)

**后续计划**:
- 等待 GitHub Actions 提供公共 ARM64 runner
- 或使用 self-hosted runner

---

## 2026-02-12 - v1.5 macOS ARM64 Java 8 使用 Zulu

### ✅ 修复 macOS ARM64 Java 8 构建

**问题**: Eclipse Temurin 不支持 macOS ARM64 的 Java 8（最低版本为 11.0.15）。

**解决方案**: 
- macOS ARM64 + Java 8: 使用 Azul Zulu
- 其他所有组合: 继续使用 Eclipse Temurin

---

## 2026-02-12 - v1.4 修复 macOS sha256sum 命令

### ✅ 使用跨平台兼容的校验和命令

**问题**: macOS 没有 `sha256sum` 命令。

**解决方案**: 
- 自动检测命令可用性
- Linux: 使用 `sha256sum`
- macOS: 使用 `shasum -a 256`

---

## 2026-02-12 - v1.0 初始配置

### 特性

- 支持 Java 8, 11, 17, 21, 23
- 支持 macOS (x64, aarch64)
- 支持 Linux (x64)
- 支持 Windows (x64)
- 自动生成 SHA256 校验和
- Git tag 触发构建
- 自动创建 Release

---

本更新日志遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/) 格式。
