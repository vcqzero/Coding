# Apm的使用

apm是基于elasticsearch 和kibaba服务的；

通过docker-compose 进行安装

官方安装文档 https://www.elastic.co/guide/en/apm/get-started/current/quick-start-overview.html



## APM SERVER

### 安装和使用

https://www.elastic.co/guide/en/apm/server/current/running-on-docker.html

[配置](https://www.elastic.co/guide/en/apm/server/7.9/configuration-template.html)

## 运行

```shell
python -m elastalert.elastalert --verbose --rule example_frequency.yaml
```



### 添加可搜索字段

#### span.http.response.status_code

I first had to overwrite the template with a new one containing the `span.http.response.status_code` field by adding this to my apm-server.yml:

```yaml
setup.template.overwrite: true
setup.template.append_fields:
- name: span.http.response.status_code
  type: long
```

文档 :https://discuss.elastic.co/t/unable-to-visualize-span-http-response-status-code/246522



## Rule Type

## 报警配置

### 邮箱报警

文档：https://elastalert.readthedocs.io/en/latest/ruletypes.html#email

```yaml
# 报警配置
alert:
  - debug
  - email:
      email: 1250857765@qq.com

from_addr: 1250857765@qq.com
smtp_host: smtp.qq.com
smtp_port: 587
smtp_auth_file: ./smtp_auth_file.yaml
```

```yaml
# smtp_auth_file.yaml 文件
user: 1250857765@qq.com
password: argynywksrkwiaeg
```

```yaml
alert_subject_args:
    - labels.group
    - service.name
    - service.environment
    - error.grouping_key
  alert_text_args:
    - service.name
    - service.environment
    - error.grouping_key
    - error.exception[0]message
    - error.culprit
  alert_subject: '[监控][ES-APM][{0}] 错误监控 {1} {2} {3}'
  alert_text: |-
    {3}
    {4}
    http://localhost:5601/app/apm#/services/{0}/errors/{2}?rangeFrom=now-7d&rangeTo=now&environment={1}
```

```yaml
# 报警配置
alert:
  - debug
  - email:
      email: 1250857765@qq.com

from_addr: 1250857765@qq.com
smtp_host: smtp.qq.com
smtp_port: 587
smtp_auth_file: ./smtp_auth_file.yaml

alert_subject_args:
    - service.name
alert_text_args:
    - service.name
alert_subject: '[监控][ELASTALERT_CLUSTER][{0}] 错误监控 页面加载超时告警'
alert_text: |-
    {0}
    在5分钟内，国内用户访问页面时，加载时间超过20s的事件至少发生了50次，请排查！
    http//localhost:5601/app/apm#/services/{0}/transactions?kuery=client.geo.country_iso_code:CN&transactionType=page-load&rangeFrom=now-1d&rangeTo=now
```

