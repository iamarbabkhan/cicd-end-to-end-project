
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
apt install unzip
adduser sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
```
