---
layout: post
title: Bitbucket Pipelines deploy to Azure Stock Standard Tomcat Webapp
---

<tl;dr>

```
image: maven:3.3.9

pipelines:
  default:
    - step:
        script:
          - mvn -f my-service/pom.xml clean install
          - cp /opt/atlassian/pipelines/agent/build/my-service/target/my-service.war /opt/atlassian/pipelines/agent/build/my-service/target/ROOT.war
          - apt-get update
          - apt-get -qq install ncftp
          - ncftpput -v -u "$FTP_USERNAME" -p "$FTP_PASSWORD" -R $FTP_SERVER /site/wwwroot/webapps /opt/atlassian/pipelines/agent/build/my-service/target/ROOT.war
```
