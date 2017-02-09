---
layout: post
title: azure, tomcat, bitbucket, pipelines, ci, deploy
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

* above file is called `bitbucket-pipelines.yml` and needs to be placed in the root of the git repository in bitbucket, [pipelines](https://confluence.atlassian.com/bitbucket/configure-bitbucket-pipelines-yml-792298910.html) must be enabled.
* [pipelines](https://confluence.atlassian.com/bitbucket/configure-bitbucket-pipelines-yml-792298910.html) uses a base docker image but you can specify one that has what you need.
  * [maven](http://maven.apache.org/) is what I needed.
* tomcat does [auto deployment stuff](https://tomcat.apache.org/tomcat-8.0-doc/config/host.html#Automatic_Application_Deployment) if you name the war file and put it in the expected place.
  * hence rename war file to ROOT.war
  * hence `ftp` the file to `/site/wwwroot/webapps` on the azure webapp.
* azure web apps provide `ftp` and `ftps` to enable deployment for continuous deployment tools. 
* [ncftp](http://www.ncftp.com/ncftp/) is easier to script than vanilla `ftp`
* as per the [ncftpput documentation](http://www.ncftp.com/ncftp/doc/ncftpput.html) 
  * `ncftpput [options] remote-host remote-directory local-files...`
  * `-v` disables progress meter
  * `-u` enables the specification of a username
  * `-p` enables the specification of a password
  * `-R` does a recursive mode copy; copy whole directory tree.. (hmmmm..)
* bitbucket pipelines provide environment variables to secure passwords etc
  
