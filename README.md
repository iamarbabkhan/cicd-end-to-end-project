
### Ultimate CI/CD end to end pipeline
clone the repository from github
```
git clone https://github.com/iamarbabkhan/cicd-end-to-end-project.git
```
Pre-Requisites:
- Java (JDK)
Install Java

```
sudo apt update
sudo apt install default-jre
```
![Screenshot-from-2024-03-02-22-40-40.png](https://i.postimg.cc/jS3NT0Z8/Screenshot-from-2024-03-02-22-40-40.png)

Verify Java is Installed
```
java -version
```
install jenkins
```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
```
![Screenshot-from-2024-03-02-22-51-35.png](https://i.postimg.cc/jq6xppXy/Screenshot-from-2024-03-02-22-51-35.png)

enable the Jenkins service
```
sudo systemctl enable jenkins
```
start the Jenkins service
```
sudo systemctl start jenkins
```
check the status of the Jenkins service
```
sudo systemctl status jenkins
```
access jenkins using `localhost:8080`
Copy password
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![Untitled-design.png](https://i.postimg.cc/d13r0gR0/Untitled-design.png)

now Jenkins is ready to use
![image](https://i.postimg.cc/RZXvdtg9/image.png)
- Go to dashboard
- Select new item
![Screenshot-from-2024-03-04-23-35-39.png](https://i.postimg.cc/W4PT2PXW/Screenshot-from-2024-03-04-23-35-39.png)
![Screenshot-from-2024-03-05-00-03-50.png](https://i.postimg.cc/pLBqdwvV/Screenshot-from-2024-03-05-00-03-50.png)
- Go to manage jenkins -> plugins -> available plugins
- download docker pipeline plugin
![Screenshot-from-2024-03-05-00-07-28.png](https://i.postimg.cc/43D52sb5/Screenshot-from-2024-03-05-00-07-28.png)
download sonarqube scanner plugin
![Screenshot-from-2024-03-05-00-11-43.png](https://i.postimg.cc/1tc970N3/Screenshot-from-2024-03-05-00-11-43.png)
Configure a Sonar Server
```
sudo su -
adduser sonarqube
apt install unzip
sudo su - sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
```
![Screenshot-from-2024-03-05-17-38-07.png](https://i.postimg.cc/5NRc0Kvv/Screenshot-from-2024-03-05-17-38-07.png)
- now sonarqube will be access from localhost:9000
![Screenshot-from-2024-03-05-17-41-13.png](https://i.postimg.cc/QCw5zr9h/Screenshot-from-2024-03-05-17-41-13.png)
Generate the tokens to Authenticate Jenkins with sonalqube
- My account -> Security -> Generate
![image-1.png](https://i.postimg.cc/FR4887ks/image-1.png)
- Copy the token
- go to jenkins -> manage jenkins -> credential
- go to systems -> global credentials -> add credentials
- paste the tokens on **secret** section
![Screenshot-from-2024-03-05-18-01-33.png](https://i.postimg.cc/QCM10Zsx/Screenshot-from-2024-03-05-18-01-33.png)

add dockerhub credential
![Screenshot-from-2024-03-05-23-36-52.png](https://i.postimg.cc/856XF7J6/Screenshot-from-2024-03-05-23-36-52.png)
add github credential
- i have generated tokens in github and paste inside secret section in jenkins
![Screenshot-from-2024-03-05-23-44-23.png](https://i.postimg.cc/76q3Mbbc/Screenshot-from-2024-03-05-23-44-23.png)
Jenkins job succesfully executed
![Screenshot-from-2024-03-10-20-18-06.png](https://i.postimg.cc/mrhrxfzK/Screenshot-from-2024-03-10-20-18-06.png)
source code quality check passed in sonarqube
![Screenshot-from-2024-03-10-20-19-05.png](https://i.postimg.cc/xCTc1J4p/Screenshot-from-2024-03-10-20-19-05.png)
succesfully build docker image and pushed it to dockerhub
![Screenshot-from-2024-03-10-20-33-55.png](https://i.postimg.cc/pTQF1Mnz/Screenshot-from-2024-03-10-20-33-55.png)
### CI(continious integration) part is completed
```
sudo su -
sudo apt install docker.io -y
usermod -aG docker jenkins
usermod -aG docker arbab
systemctl restart docker
```
Install Kubectl
```
sudo apt-get install -y apt-transport-https ca-certificates curl
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
sudo apt-get update
sudo apt-get install -y kubectl
```
Install Minilkube which will create local Kubernetes cluster
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
start and create kubernetes local cluster

```
minikube start --driver=docker
```
![Screenshot-from-2024-03-05-22-08-52.png](https://i.postimg.cc/bvySknB2/Screenshot-from-2024-03-05-22-08-52.png)

Install Argo CD using kubernets operator
- [OperatorHub](https://operatorhub.io/operator/argocd-operator)
```
curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.27.0/install.sh | bash -s v0.27.0
kubectl create -f https://operatorhub.io/install/argocd-operator.yaml
```
check running pods in the operators namespaces
```
kubectl get pods -n operators
- Argo CD is running perfectly
```
![Screenshot-from-2024-03-05-22-27-33.png](https://i.postimg.cc/MTDqQWT6/Screenshot-from-2024-03-05-22-27-33.png)
Create a new Argo CD cluster
![Screenshot-from-2024-03-10-22-15-16.png](https://i.postimg.cc/gjQj6KJ0/Screenshot-from-2024-03-10-22-15-16.png)
```
kubectl apply -f argocd-basics.yml
```
![Screenshot-from-2024-03-10-22-18-56.png](https://i.postimg.cc/gJBT9g44/Screenshot-from-2024-03-10-22-18-56.png)
to see argocd pods is created
![Screenshot-from-2024-03-10-22-23-44.png](https://i.postimg.cc/T3L7mjdW/Screenshot-from-2024-03-10-22-23-44.png)
to run argocd on my browse
```
kubectl edit svc example-argocd-server
```
edit `example-argocd-server` service and change `type:ClusterIp` to `NodePort`
