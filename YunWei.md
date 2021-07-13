# 系统
##  Mysql
### 查看mysql相关信息

- 查看数据库进程 ： `show processlist;`
   或者 `/work/servers/mysql/bin/mysqladmin -u root processlist`

- 查看数据库事务信息 ： `SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;`

- 查看当前锁定的事务 ： `SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS;`

- 查看当前等锁的事务 ： `SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;`

- 查看所有表的大小： `SELECT CONCAT(table_schema,'.',table_name) AS 'Table Name', CONCAT(ROUND(table_rows/1000000,4),'M') AS 'Number of Rows', CONCAT(ROUND(data_length/(1024*1024*1024),4),'G') AS 'Data Size', CONCAT(ROUND(index_length/(1024*1024*1024),4),'G') AS 'Index Size', CONCAT(ROUND((data_length+index_length)/(1024*1024*1024),4),'G') AS'Total'FROM information_schema.TABLES;`

### 导库操作

- Innodb表的导库操作
  `mysqldump -uroot -p -h127.0.0.1 --single-transaction --dump-slave=2 -B $db --tables 表名> /tmp/${db}.sql `

- MyISM表的导库操作
  `mysqldump -uroot -p -h127.0.0.1 --lock-all-tables --dump-slave=2 -B $db --tables 表名> /tmp/${db}.sql`


## Linux
### 升级内核
```
sudo wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && sudo chmod +x bbr.sh && sudo ./bbr.sh
```

### 检查网络状态
- Traceroute
- iperf
- mtr

### 时间同步
`sudo ntpdate -u time.pool.aliyun.com`

### 四层转发

`ssh -C -f -N -g -L 0.0.0.0:中转端口:目的IP:目的端口 连接用户@中转IP`


# Jenkins
## Jenkins迭代流程

1. [GiteLab_jjb-template]() 存储了jenkins job的templete文件
   1. 根据tpl文件内的字段`pipeline-scm`，定义pipline文件的位置
      * SaaS的CI发布pipline文件位置 [GiteLab_pipeline]()，其中有三个分支，master，ci，cd
   2. 根据`job-template.id`字段标识，在具体job信息中通过`import`方法调用
2. [GiteLab_jjb]()存储了job文件，通过hook通知[Jenkins_job-update]()调用进行配置信息的迭代，部分项目需要手动触发构建
3. [Jenkins_job-update]()根据hook请求来的`jenkins-jobs --conf jenkins_jobs.ini update --workers 5`文件调用docker完成对[Jenkins_work]()内job配置的更新



##  JJB基本原理

Jenkins Job Builder 基本组成原理

* 完整jenkins job组成

  * job配置文件，记录调用templete文件的id，env配置信息

    ```yaml
    - !include: jenkins/defaults.yml
    - !include: jenkins/templates/ci/template.yml
    
    - project:
        name: TEST
        folder: TEST
        jobs:
          - template_pipeline:
              job_name: '{folder}/{job_prefix}-test'
              env_vars: |
                key=value
    ```

    * `#!include: jenkins/templates/ci/template.yml` 引用的的templete的文件位置
    * `template_pipeline` 调用的templete的id，即第八行
    * `folder: TEST`、`env_vars: ` 都是配置信息

  * templete配置文件，记录调用pipline的相关信息

    ```yaml
    job-template: 
        id: template_pipeline
        name: '{job_name}'
        project-type: pipeline
        description: '{description|sample}'
        git_branches: 
            - ci
    
        pipeline-scm:
            scm:
              - git:
                  url: '{git_server_base_url|http://GiteLab_pipeline/'
                  branches: "{git_branches}"
                  shallow-clone: true
            script-path: jenkinsfile/Jenkinsfile.{jenkinsfile}
            lightweight-checkout: true
    ```

    * `id: template_pipeline` 作为templete的唯一标识id，用于job引用
    * `pipeline-scm:` 引用pipline的方法，目前采取的是调用 [GiteLab_pipeline]() 的指定pipline配置文件
    * `git_branches:` 读取 [GiteLab_pipeline]()的哪个分支，(master、ci 、cd)
    * `script-path: jenkinsfile/ci/Jenkinsfile.{jenkinsfile}` 存储pipline的文件在git中的位置

  * pipline文件，实现指定的工作流流程

* Jenkins的查看层级，server -> view -> project -> job


## Jenkins工作流基本方法

``` groovy
def node=""
if (env.node){
    node =env.node
}else{
    node ="develop"
}

def maven_image=""
if (env.maven_image){
    maven_image =env.maven_image
}else{
    maven_image ="docker.io/maven:3.3.9"
}

pipeline {
    agent {label node}
    agent {docker {
            image "${maven_image}"
            label node
            args "${docker_args}"
          }}
    options {
	    timeout(time: 1, unit: 'HOURS')
    }
    stages {
        stage('delete workspace') {
            steps{
                deleteDir()
            }
        }
        stage('checkout') {
            steps{
                script{
                    }
                }
            }
        }
		stage('build') {
		    steps{
				script{

				}
			}
            post{
                always{
                    script{
                        if (env.DNET_PRODUCT){
                            build job: 'sourcecode_scan', parameters: [string(name: 'url', value: BUILD_URL),string(name: 'product', value: "")], wait: false
                        }
                }
        
                failure{
                    script{
                        def subject = env.email_build_subject ? "${env.email_build_subject}" : "构建失败-${env.JOB_NAME}"
                        def to = env.email_build_recipients ? "${env.email_build_recipients}" : "heweiwei@hd123.com"
                        def body = env.email_build_body ? "${env.email_build_body}" : "${env.JOB_NAME}构建失败,更多信息请查阅：${BUILD_URL}"
                        // send email
                        emailext subject: subject, to: to, body:body,attachLog:true
                    }
                }
            }
		}
    }
    post{
        success{
            script{
                if (env.post_shell){
                    sh """
                    ${env.post_shell}
                    """
                }
            }
        }
    }
}

```

* `build job` 调用创建新的job进行处理
* `triggerRemoteJob job` 调用新job 