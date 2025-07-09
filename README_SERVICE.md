# 电费监控服务 (Linux Service)

这是一个基于Linux systemd的电费监控服务，可以定时查询宿舍电费并在电量不足时发送通知。

## 功能特性

- 🕐 **定时监控**: 每天晚上7点自动查询电费
- 🔔 **多种通知**: 支持Server酱和自定义Webhook通知
- 📊 **灵活配置**: 使用YAML配置文件，易于管理
- 🛡️ **安全运行**: 作为系统服务运行，支持自动重启
- 📝 **详细日志**: 完整的日志记录和监控

## 系统要求

- Linux系统 (支持systemd)
- Python 3.6+
- pip3

## 安装步骤

### 1. 下载文件

确保以下文件在同一目录：
- `power_monitor_service.py` - 主程序
- `config.yaml` - 配置文件
- `dorm_rooms_2025.csv` - 宿舍数据
- `power-monitor.service` - systemd服务文件
- `install.sh` - 安装脚本

### 2. 运行安装脚本

```bash
sudo chmod +x install.sh
sudo ./install.sh
```

### 3. 配置服务

编辑配置文件：
```bash
sudo nano /etc/power-monitor/config.yaml
```

主要配置项：
- `monitor.schedule_time`: 监控时间 (默认19:00)
- `notifications.server_chan.sendkey`: Server酱的SENDKEY
- `dormitories`: 要监控的宿舍列表

### 4. 启动服务

```bash
sudo power-monitor-start
```

## 使用方法

### 基本命令

```bash
# 启动服务
sudo power-monitor-start

# 停止服务
sudo power-monitor-stop

# 查看服务状态
sudo power-monitor-status

# 查看实时日志
sudo power-monitor-logs

# 立即执行一次监控
sudo power-monitor-run
```

### 配置文件说明

`/etc/power-monitor/config.yaml`:

```yaml
# 监控设置
monitor:
  schedule_time: "19:00"  # 每天监控时间
  global_threshold: 10.0   # 全局电量阈值

# 通知设置
notifications:
  server_chan:
    enabled: true
    sendkey: "YOUR_SENDKEY_HERE"

# 宿舍列表
dormitories:
  - dorm_id: "101640017"
    dorm_name: "1号楼-101"
    dorm_type: "1"
    low_power_threshold: 10.0
    enabled: true
```

### 添加监控宿舍

1. 编辑配置文件：
```bash
sudo nano /etc/power-monitor/config.yaml
```

2. 在 `dormitories` 部分添加宿舍信息：
```yaml
dormitories:
  - dorm_id: "宿舍ID"
    dorm_name: "宿舍名称"
    dorm_type: "宿舍类型"
    low_power_threshold: 10.0  # 电量阈值
    enabled: true
    description: "描述信息"
```

3. 重启服务：
```bash
sudo power-monitor-stop
sudo power-monitor-start
```

## 日志查看

### 查看服务日志
```bash
sudo power-monitor-logs
```

### 查看系统日志
```bash
sudo journalctl -u power-monitor.service
```

### 查看特定日期的日志
```bash
sudo journalctl -u power-monitor.service --since "2024-01-01"
```

## 故障排除

### 1. 服务无法启动

检查服务状态：
```bash
sudo power-monitor-status
```

查看详细错误：
```bash
sudo journalctl -u power-monitor.service -n 50
```

### 2. 配置文件错误

验证YAML语法：
```bash
python3 -c "import yaml; yaml.safe_load(open('/etc/power-monitor/config.yaml'))"
```

### 3. 网络连接问题

测试网络连接：
```bash
curl -I http://hydz.xsyu.edu.cn/wxpay/homeinfo.aspx
```

### 4. 通知发送失败

检查SENDKEY配置：
```bash
grep sendkey /etc/power-monitor/config.yaml
```

测试通知：
```bash
sudo power-monitor-run
```

## 卸载服务

```bash
sudo ./install.sh uninstall
```

## 手动运行

如果不想安装为系统服务，也可以手动运行：

```bash
# 安装依赖
pip3 install -r requirements_service.txt

# 立即执行一次
python3 power_monitor_service.py --once

# 启动服务模式
python3 power_monitor_service.py
```

## 安全注意事项

1. **文件权限**: 确保配置文件和日志文件权限正确
2. **用户权限**: 服务以专用用户运行，避免权限过高
3. **网络安全**: 确保网络连接安全，避免敏感信息泄露
4. **日志管理**: 定期清理日志文件，避免磁盘空间不足

## 更新服务

1. 停止服务：
```bash
sudo power-monitor-stop
```

2. 备份配置文件：
```bash
sudo cp /etc/power-monitor/config.yaml /etc/power-monitor/config.yaml.backup
```

3. 更新文件并重新安装：
```bash
sudo ./install.sh
```

4. 恢复配置：
```bash
sudo cp /etc/power-monitor/config.yaml.backup /etc/power-monitor/config.yaml
```

5. 启动服务：
```bash
sudo power-monitor-start
```

## 许可证

本项目基于MIT许可证开源。 