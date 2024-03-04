
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
