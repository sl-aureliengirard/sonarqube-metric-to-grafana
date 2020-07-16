## SHOW SONARQUBE METRIC TO GRAFANA
---

**stack**:  sonarqube、influxdb、grafana、docker


####  Data flow diagram

![picture](https://github.com/qinrui777/sonarqube-metric-to-grafana/blob/master/images/Data-flow-diagram.png)
#### Install influxdb
TBD

#### Install grafana
TBD

### Make data collector
```bash
#build a image for collector
cd sonar_collector_docker
docker build -t export-sonarqube-metric:v0.1 .
```

```bash
docker run -d --name export-sonarqube \
-e SONAR_BASE_URL=http://sonar.yourGroup.com:9876 \
-e SONAR_USER=admin \
-e SONAR_PASSWORD=admin \
-e INFLUX_URL=192.168.xx.xxx \
-e INFLUX_USER=admin \
-e INFLUX_PASSWORD=admin \
-e INFLUX_DB=sonarqube_exporter \
-e INTERVAL=86400 <YourDockerRepo>/export-sonarqube-metric:v7
```
you can change the environment,like `INTERVAL` which means how long to collect the data from sonarqube server. 

#### Add alert in grafana 

edit  `grafana.ini` file  
```bash
#################################### SMTP / Emailing ##########################
[smtp]
enabled = true
host = smtp.qq.com:465
user = 305xxxxx18@qq.com
# If the password contains # or ; you have to wrap it with trippel quotes. Ex """#password;"""
#qq 邮箱的客户端授权码
password = wrxxxxxxaaaaacdd
;cert_file =
;key_file =
;skip_verify = false
from_address = 305xxxxx18@qq.com
;from_name = Grafana
# EHLO identity in SMTP dialog (defaults to instance_name)
;ehlo_identity = dashboard.example.com
```

```bash
#################################### Alerting ############################
[alerting]
# Disable alerting engine & UI features
;enabled = true
# Makes it possible to turn off alert rule execution but alerting UI is visible
execute_alerts = true
```

---
More ref:
- https://grafana.com/grafana/dashboards/10763
- [telegraf+influxdb+grafana环境初探
](https://www.xujun.org/note-4822.html)