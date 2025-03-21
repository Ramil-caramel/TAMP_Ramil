##Laboratory work VIII
Данная лабораторная работа посвещена изучению систем автоматизации развёртывания и управления приложениями на примере Docker
```
$ export GITHUB_USERNAME=<имя_пользователя>
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
$ git clone https://github.com/${GITHUB_USERNAME}/lab07 lab08
$ cd lab08
$ git submodule update --init
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08
```
делаем то же самое что и в предыдущих лабортаорных
Заиписываем конфигурацию нашего докер файла
```bash
$ docker build -t logger .
```
запускаем сборку докера
