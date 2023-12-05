# 通用服务器监控服务部署模板(Docker Compose版本)
---
该项目为以Docker Compose方式的基于Prometheus+Cadvisor+Grafana的服务器监控部署模版

> 该模板目前仅支持Linux系列操作系统，Windows与Mac等系列暂时不可使用

## 准备工作
### 1. 编辑`.env-dist`
修改`ADMIN_USER`、`ADMIN_PASSWORD`为实际使用的账户密码。
如果本地使用了多套该compose运行环境，则必须调整各自目录下该文件的`COMPOSE_PROJECT_NAME`参数值，以确保相互间唯一

### 2. 创建`docker-compose.yml`
```bash
cp docker-compose.yml.template docker-compose.yml
```

> 若需要使用`Alertmanager`，则需开启`alertmanager`相关配置

### 3. 修改`prometheus/prometheus.yml`(非必须)
根据实际需求，可配置额外的采集配置`scrape_configs`

> 若需要使用`Alertmanager`，则需开启其相关配置

### 4. 修改`prometheus/alert_rules.yml`(非必须)
根据实际需求，可配置额外的报警规则

### 5. 修改`alertmanager/alertmanager.yml`(非必须)
配置收发规则与邮件邮箱

### 6. 创建`/opt/data/prometheus`目录并调整权限
```
# 参考 https://stackoverflow.com/a/69032349
mkdir -p /opt/data/prometheus
chown 65534:65534 /opt/data/prometheus
```

---
## Build
`make build`

## Start
`make start`

## Stop
`make stop`

## Status
`make status`

## Down
`make down`

---
## Grafana使用
1. 添加Prometheus数据源
Connections -> Data sources -> Add new data source -> 选择Prometheus
设置Prometheus server URL 为 `http://prometheus:9090` 然后保存
2. 添加Dashboards仪表板
Dashboards -> import -> 导入仪表板
可用的仪表版模板：
    - [Node Exporter Dashboard 220417 通用Job分组版](https://grafana.com/grafana/dashboards/16098-1-node-exporter-for-prometheus-dashboard-cn-0417-job/)
    - [Docker monitoring with service selection](https://grafana.com/grafana/dashboards/15798-docker-monitoring-with-service-selection/)

    若无法访问，其离线json文件在`dashboards`目录下，可直接导入至Prometheus
3. 设置首页默认仪表板
Administration -> Default preferences -> 选择Home Dashboard
4. 语言设置
用户头像 -> Profile -> 选择Language
