### 09.03_Jenkins
============================================</br>
Установка Jenkins на Ubuntu 20.04: `apt update && apt install openjdk-8-jre`, далее вставить команды в PuTTY из [jenkins.io/doc](https://www.jenkins.io/doc/book/installing/linux/) </br>
Jenkins доступен: `localhost: 8080:8080` </br>
Выбор плагинов: Git, GitHub, GitParameter, GitHub Branch, SSH, SSHAgent, Pipeline </br>
Jenkins login в Git по SSH: на хосте с jenkins выполнить -> keygen -t rsa, cat ~/.key.pub -> добавить в https://github.com/settings/keys </br>
В Jenkins: Настроить jenkins -> Manage Creditionals -> add creditionals -> SSH Username with private key -> Private Key. Задать Username - Git_login.</br>
Passphrase задавать не нужно. </br>
Создать Job Freestile -> ok, Управление исходным кодом - Git, Repository URL: git@github.com:murzinvit/example-playbook.git, в поле Credentials выбрать - Git_login </br>



