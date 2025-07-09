# 电费监控服务 - Linux版本

这是一个专门为Linux系统设计的电费监控服务，可以定时查询宿舍电费并在电量不足时发送通知。

## 文件说明

- `power_monitor_service.py` - 主程序文件
- `config.yaml` - 配置文件（YAML格式）
- `dorm_rooms_2025.csv` - 宿舍数据文件
- `power-monitor.service` - systemd服务文件
- `install.sh` - 自动安装脚本
- `requirements_service.txt` - Python依赖文件
- `README_SERVICE.md` - 详细使用说明

## 快速开始

1. 将整个文件夹上传到Linux服务器
2. 运行安装脚本：
   ```bash
   sudo chmod +x install.sh
   sudo ./install.sh
   ```
3. 编辑配置文件：
   ```bash
   sudo nano /etc/power-monitor/config.yaml
   ```
4. 启动服务：
   ```bash
   sudo power-monitor-start
   ```

## 主要特性

- 🕐 **定时监控**: 每天晚上7点自动查询电费
- 🔔 **多种通知**: 支持Server酱和自定义Webhook通知
- 📊 **灵活配置**: 使用YAML配置文件，易于管理
- 🛡️ **安全运行**: 作为系统服务运行，支持自动重启
- 📝 **详细日志**: 完整的日志记录和监控

## 详细文档

请查看 `README_SERVICE.md` 文件获取完整的使用说明。

## 系统要求

- Linux系统 (支持systemd)
- Python 3.6+
- pip3 