# beanstalk_newrelic_test
참고: https://docs.newrelic.com/kr/docs/infrastructure/install-infrastructure-agent/config-management-tools/configure-infrastructure-agent-aws-elastic-beanstalk/

- 샘플 코드 다운로드
    - https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/tutorials.html
    - nodejs 에러나서 python으로 다운로드
- 코드 안에 `.ebextensions/newrelic.config` 파일 생성 후 압축

![image](https://github.com/jaeeuncho34/beanstalk_newrelic_test/assets/60213218/5661511b-0457-43d3-9fca-cbfdad1e1b65)


```yaml
files:
  "/etc/newrelic-infra.yml" :
    mode: "000644"
    owner: root
    group: root
    content: |
      license_key: <LICENSE_KEY>

commands:
# Create the agent’s yum repository
  "01-agent-repository":
    command: sudo curl -o /etc/yum.repos.d/newrelic-infra.repo https://download.newrelic.com/infrastructure_agent/linux/yum/amazonlinux/2023/x86_64/newrelic-infra.repo
#
# Update your yum cache
  "02-update-yum-cache":
    command: yum -q makecache -y --disablerepo='*' --enablerepo='newrelic-infra'
#
# Run the installation script
  "03-run-installation-script":
    command: sudo yum install newrelic-infra -y
```

## troubleshooting (MAC)

- 코드 수정 후 zip 파일을 생성하면 지저분하게 나옴
    - 해당 디렉터리로 이동 후 명령어 넣기
        
        ```yaml
        zip -r last1.zip ./*
        # 숨김파일 추가
        zip -r last1.zip ./.*
        ```
        
    - 정상적으로 압축되면 아래처럼 나옴

```yaml
" zip.vim version v32
" Browsing zipfile /Users/mzc01-jaecho/Desktop/elasticbeanstalk/last1.zip
" Select a file with cursor and press ENTER

EBSampleApp-Python.iml
application.py
cron.yaml
.ebextensions/
.ebextensions/00sample_log.config
.ebextensions/newrelic.config
```
