## Pipeline Jenkins Backend, Integrasi Webhook, dan Notifikasi Telegram

### Step 1

Tambahkan `Jenkinsfile` didalam folder repository Backend. Isikan berikut

```
def branch = "master"
def remoteurl = "https://github.com/yuuzukatsu/dumbflix-backend.git"
def remotename = "jenkins"
def workdir = "~/dumbflix-backend/"
def ip = "116.193.191.120"
def username = "kelompok1"
def imagename = "dumbflix-backend"
def dockerusername = "yuuzukatsu"
def sshkeyid = "app-server"

pipeline {
    agent any

    stages {
        stage('Pull From Backend Repo') {
            steps {
                sshagent(credentials: ["${sshkeyid}"]) {
                    sh """
                        ssh -l ${username} ${ip} <<pwd
                        cd ${workdir}
                        git remote add ${remotename} ${remoteurl} || git remote set-url ${remotename} ${remoteurl}
                        git pull ${remotename} ${branch}
                        pwd
                    """
                }
            }
        }
            
        stage('Build Docker Image') {
            steps {
                sshagent(credentials: ["${sshkeyid}"]) {
                    sh """
                        ssh -l ${username} ${ip} <<pwd
                        cd ${workdir}
                        docker build -t ${imagename}:${env.BUILD_ID} .
                        pwd
                    """
                }
            }
        }
            
        stage('Deploy Image') {
            steps {
                sshagent(credentials: ["${sshkeyid}"]) {
                    sh """
                        ssh -l ${username} ${ip} <<pwd
                        cd ${workdir}
                        docker compose down
                        docker tag ${imagename}:${env.BUILD_ID} ${imagename}:latest
                        docker compose up -d
                        pwd
                    """
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sshagent(credentials: ["${sshkeyid}"]) {
			sh """
				ssh -l ${dockerusername} ${ip} <<pwd
				docker tag ${imagename}:${env.BUILD_ID} ${dockerusername}/${imagename}:${env.BUILD_ID}
				docker tag ${imagename}:latest ${dockerusername}/${imagename}:latest
				docker image push ${dockerusername}/${imagename}:latest
				docker image push ${dockerusername}/${imagename}:${env.BUILD_ID}
				docker image rm ${dockerusername}/${imagename}:latest
				docker image rm ${dockerusername}/${imagename}:${env.BUILD_ID}
				docker image rm ${imagename}:${env.BUILD_ID}
				pwd
			"""
		}
            }
        }


        stage('Send Success Notification') {
            steps {
                sh """
                    curl -X POST 'https://api.telegram.org/bot${env.telegramapi}/sendMessage' -d \
		    'chat_id=${env.telegramid}&text=Build ID #${env.BUILD_ID} Backend Pipeline Successful!'
                """
            }
        }

    }
}
```

### Step 2

Buka url Jenkins lewat url yang sudah dibuat sebelumnya. Buat pipeline
baru

![image1](https://user-images.githubusercontent.com/67664879/190385667-885f6261-ca6a-45e3-860b-bf1b02790f33.png)


### Step 3

Isikan dengan parameter berikut\
GitHub hook trigger for GITScm polling
```
- Definition : Pipeline script from SCM
- SCM : Git
- Repository URL : https://github.com/yuuzukatsu/dumbflix-backend.git
- Branches : */master
- Script Path : Jenkinsfile
```
### Step 4

Buka repository github, di menu settings tambahkan webhook

![image2](https://user-images.githubusercontent.com/67664879/190385684-ec4c537d-1888-4699-a13a-feecd3c41e0c.png) 

![image3](https://user-images.githubusercontent.com/67664879/190385694-72a879c1-5868-490c-8a27-af83a1ea7641.png)

![image4](https://user-images.githubusercontent.com/67664879/190385702-db934dc3-5a0f-4286-a1d9-e77d4ea55c5f.png)

### Step 5

Pada Payload URL, isikan url domain jenkins lalu tambahkan akhiran
`/github-webook`
```
- Payload url : <url\/github-webhook
- Content Type : json
- Event : Just push event
```
![image5](https://user-images.githubusercontent.com/67664879/190385722-3bb88879-988f-46fb-a363-bee23a7ffea1.png) 

### Step 6

Create bot telegram dengan pm ke user `@BotFather` di telegram

![image6](https://user-images.githubusercontent.com/67664879/190385729-92d0131e-6dd0-4852-8ad3-c85161748403.png)

### Step 7

Copy API yang diberikan, lalu buka kembali pipeline yang dibuat
sebelumnya. Tambahkan variable environtment baru

![image7](https://user-images.githubusercontent.com/67664879/190385741-28dd5e52-bf20-4ea8-bf9b-c59d0de939e5.png)

![image8](https://user-images.githubusercontent.com/67664879/190385751-7f448535-02b5-4540-89a1-724ac12efbde.png)
