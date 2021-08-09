### 09.03_Jenkins
============================================</br>
Сделать fork репозитория [example-playbook](https://github.com/aragastmatb/example-playbook) - на github.com справа кнопка - fork
Установка Jenkins на Ubuntu 20.04: `apt update && apt install openjdk-8-jre`, далее вставить команды в PuTTY из [jenkins.io/doc](https://www.jenkins.io/doc/book/installing/linux/) </br>
Jenkins доступен: `localhost: 8080:8080` </br>
Выбор плагинов: Git, GitHub, GitParameter, GitHub Branch, SSH, SSHAgent, Pipeline </br>
Jenkins login в Git по SSH: на хосте с jenkins выполнить -> `keygen -t rsa, cat ~/.key.pub` -> добавить в `https://github.com/settings/keys` </br>
В Jenkins зайти в: `Настроить jenkins` -> `Manage Creditionals` -> `add creditionals` -> `SSH Username with private key` -> `Private Key`</br>
В поле - `Username` указать login github аккаунта. В поле `ID` - указать любое удобное имя(Git_Hub_login).`Passphrase` задавать не нужно т.к не завали </br>
Создать `Job Freestile` -> ok, Управление исходным кодом - `Git`, `Repository URL`: git@github.com:murzinvit/example-playbook.git, </br> 
в поле `Credentials` выбрать - Git_Hub_login </br>
При выполнении Job jenkins склонирует репозиторий локальную папку и выполнит указанные shell команды. </br> 
В поле Build(сборка) выбрать - выполнить комнаду - shell, далее указать требуемые shell команды linux. </br> 
Для выполнения [example-playbook](https://github.com/murzinvit/example-playbook) потребуются следующие команды: </br>





