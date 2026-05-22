# GeyserOptimizer

[![Modrinth](https://img.shields.io/badge/Modrinth-Download-00AF5C?logo=modrinth)](https://modrinth.com/plugin/geyseroptimizer)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Minecraft](https://img.shields.io/badge/Minecraft-1.20%2B-62b47a)](https://papermc.io/)

**GeyserOptimizer** 是一个专门用于优化和提升 Geyser 插件连接稳定性的 Minecraft 服务端插件，同时提供 Java 版玩家的连接质量监控与被动优化。

> 不依赖任何外部库，纯 Bukkit/Spigot/Paper 实现，兼容 Geyser / Floodgate（可选）。

---

## ✨ 功能特性

### 🛡️ 连接稳定性优化
- **实时监控**：定期检查 Geyser 连接状态  
- **自动重连**：检测到连接问题时自动尝试恢复  
- **被动重连**：基岩版 / Java 版玩家连接不稳定时自动执行轻量级优化  
- **健康度检测**：评估连接质量并提供报告  

### 📊 状态监控
- **连接状态**：实时显示 Geyser 连接健康状态  
- **玩家统计**：监控基岩版 / Java 版玩家在线数量  
- **连接跟踪**：跟踪每个玩家的连接稳定性与 Ping 波动  
- **性能指标**：记录连接成功率、错误次数、Ping 统计等  

### 🔧 智能管理
- **配置化管理**：通过配置文件调整所有参数  
- **管理员通知**：连接状态变化时通知在线管理员  
- **详细日志**：可选详细日志模式用于问题诊断  
- **Floodgate 兼容**：完美支持 Floodgate 离线账号系统  

### 🎯 基岩版玩家优化
- **玩家级别优化**：为连接不稳定的单个玩家执行优化  
- **即时优化**：检测到不稳定连接时立即执行优化  
- **全局优化**：当不稳定玩家比例过高时触发全局优化  
- **连接保持**：自动发送保持活跃数据包维持连接  

### 🔍 精准玩家识别
- **三层识别**：Floodgate API → Geyser 内部 Session 列表 → 玩家名特征兜底  
- **不依赖前缀**：即使 Floodgate 未设置基岩版玩家前缀，也能通过 Geyser 的 Session 管理器准确识别  
- **缓存机制**：每 30 秒从 Geyser 刷新一次基岩版玩家列表，避免频繁反射调用  

### 🖥️ Java 版玩家优化
- **Ping 监控**：每 10 秒检测一次所有 Java 版玩家的 Ping  
- **波动识别**：通过 Ping 方差识别网络波动剧烈的玩家  
- **被动优化**：检测到高延迟 / 不稳定时自动执行轻量级优化  
- **连接质量报告**：每 5 分钟生成一次 Ping 统计报告  

---

## 📥 安装步骤

1. **下载插件**  
   从 [Modrinth](https://modrinth.com/plugin/geyseroptimizer) 获取 `GeyserOptimizer.jar`。

2. **放入服务端**  
   将 `.jar` 文件复制到服务器的 `plugins/` 目录下。

3. **重启服务器**  
   执行 `/restart` 或手动重启服务器。  
   > 提示：也可以使用 `/reload confirm`，但强烈建议完全重启。

4. **配置（可选）**  
   首次启动后，配置文件将自动生成于 `plugins/GeyserOptimizer/config.yml`，按需修改后执行 `/geyseropt reload` 重载。

---

## ⚙️ 配置说明

配置文件 `config.yml` 主要参数：

```yaml
# 基础设置
check-interval: 30                # 检查间隔（秒）
max-retry-attempts: 3             # 最大重试次数
auto-restart-enabled: true        # 是否启用自动重启 Geyser
notify-admins: true               # 是否通知管理员
detailed-logging: false           # 详细日志模式

# 基岩版玩家优化
bedrock-optimization:
  passive-reconnect-enabled: true
  enable-player-optimization: true
  immediate-optimization: true
  unstable-threshold: 3              # 不稳定阈值（断开次数）
  min-unstable-players: 2            # 触发全局优化的最小不稳定玩家数
  trigger-threshold: 0.3             # 触发全局优化的比例（30%）

# Java 版玩家优化
java-player-optimization:
  enabled: true
  ping-threshold: 200                # Ping 阈值（毫秒）
  passive-reconnect-enabled: true
  unstable-threshold: 3
  min-unstable-players: 2
  trigger-threshold: 0.3
  warn-player: true                  # 是否警告玩家
  warn-interval: 30                  # 警告间隔（每 N 次不稳定事件警告一次）
  check-interval-ticks: 200          # 检查间隔（200 tick = 10 秒）
