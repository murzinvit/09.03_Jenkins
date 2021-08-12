## 09.03_Jenkins
============================================</br>
#### Подготовка к выполнению:
Сделал fork от [aragastmatb/example-playbook](https://github.com/aragastmatb/example-playbook) (на сайте github.com справа кнопка - fork) </br>
Установка Jenkins на Ubuntu 20.04: `apt update && apt install openjdk-8-jre` </br>
Далее для удобства можно копировать команды в PuTTY из документации по установке с сайта [jenkins.io](https://www.jenkins.io/doc/book/installing/linux/) </br>
После установки Jenkins доступен на: http://localhost:8080, перезапустить jenkins можно так: http://localhost:8080/restart </br>
Выбор плагинов: Git, GitHub, SSH, SSHAgent, Pipeline, Amazon EC2 plugin </br>
### Добавление ssh ключа на AWS instance: </br>
 - При создании ec2 instance в AWS назначить ему key-pair ppk для PuTTY и сохранить ppk файл </br>
 - Через PuTTY + key.ppk зайти на instance(login - ec2-user), выполнить - sudo passwd root, yum install nano git -y, nano /etc/ssh/sshd_config </br>
 - В sshd_config выставить PasswdAuthentication yes, далее установить java - yum install java-1.8.0-openjdk.x86_64,pip3 install absible далее reboot </br>
 - Теперь с локального jenkins master - ssh-keygen -t rsa, ssh-copy-id root@0.0.0.0(ip instance) </br>
Jenkins login в Git по SSH: на хосте с jenkins выполнить -> `keygen -t rsa, cat ~/.key.pub` -> добавить в `https://github.com/settings/keys` </br>
В Jenkins зайти в: `Настроить jenkins` -> `Manage Creditionals` -> `add creditionals` -> `SSH Username with private key` -> `Private Key`</br>
В поле - `Username` указать login github аккаунта. В поле `ID` - указать любое удобное имя(Git_Hub_login).`Passphrase` задавать не нужно т.к не завали </br>
Создать `Job Freestile` -> ok, Управление исходным кодом - `Git`, `Repository URL`: git@github.com:murzinvit/example-playbook.git, </br> 
в поле `Credentials` выбрать - Git_Hub_login </br>
При выполнении Job jenkins склонирует репозиторий локальную папку и выполнит указанные shell команды. </br> 
В поле Build(сборка) выбрать - выполнить комнаду - shell, далее указать требуемые shell команды linux. </br> 
Для выполнения [example-playbook](https://github.com/murzinvit/example-playbook) потребуются следующие команды: </br>
 - `ansible-vault decrypt secret --vault-pass-file vault_pass`</br>
 - `ansible-galaxy install -r requirements.yml`</br>
 - `ansible-playbook site.yml -i inventoryt/prod.yml`</br>
 При настройке job установить среду сборки - SSH Agent, выбрать credentionals:</br>
![screen](https://github.com/murzinvit/screen/blob/88276bde7cfdce0da105b6ce0d3e41a0efb9fa41/Task_SSH_Enabled.jpg)
Если slaves будут статическими, то перед их добавлением нужно настроить авторизацию ssh по ключу между master и slave:</br>
![screen](https://github.com/murzinvit/screen/blob/847495506518851559ee0ed22ec97c3f3c8fb214/add_slave.jpg)
При настройке Credentionals в поле `username` указывать реальный login name </br>
![screen](https://github.com/murzinvit/screen/blob/484f9a3a2f0357f1181e6f5e7ec1975987ff7fc7/Credentionals_jenkins.jpg)
При выполнении playbook на docker контейнере (jenkins master и docker контейнер на одной машине) выходит ошибка доступа к демону докера, для исправления: </br>
`usermod -aG docker jenkins`</br>
`usermod -aG root jenkins`</br>
`chmod 777 /var/run/docker.sock`</br>
### Declarative Pipeline, который будет выкачивать репозиторий с плейбукой и запускать её:</br>
![screen](https://github.com/murzinvit/screen/blob/2e9f1fc2adbafed776597e7ed7a602116f1e1778/Pipeline_scrypt.jpg)</br>
### Перенести Declarative Pipeline в репозиторий в файл Jenkinsfile(поменять master на main)[pipeline_repo](https://github.com/murzinvit/pipeline_repo)</br>
![screen](https://github.com/murzinvit/screen/blob/21252acc1a9917cc7b9dc59a567b884bcd630ca2/Jenkinsfile.jpg)</br>
### Создать Scripted Pipeline, наполнить его скриптом из pipeline
[ScriptedJenkinsfile](https://github.com/murzinvit/pipeline_repo/blob/main/ScriptedJenkinsfile)
### Добавление credentionals для aws login:
![ScriptedJenkinsfile](https://github.com/murzinvit/screen/blob/257e381048af8a761919b80b66df063ed613e402/Credentionals_for_aws.jpg)
### Настройка соединения с AWS cloud:
![ScriptedJenkinsfile](https://github.com/murzinvit/screen/blob/32f3e39dd1ee586c9bf453ce986adbded5212422/Cloud_connect_settings.jpg)
### В конечном итоге поднял cloud через docker на том же хосте что и jenkins master, по [этой](https://russianblogs.com/article/73231545720/) статье:</br>
Образ использовал из задания - docker.io/aragast/agent:7







