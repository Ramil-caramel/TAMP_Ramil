## Laboratory work 2

Данная лабораторная работа посвещена изучению систем контроля версий на примере Git.
## Report

```sh
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_EMAIL=<адрес_почтового_ящика>
$ export GITHUB_TOKEN=<сгенирированный_токен>
$ alias edit=<nano|vi|vim|subl>
```
Создаем переменные окружения и псевдоним функции

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```
Переходим в директорию и выполняем скрипт из первой лабараторной работы

```sh
$ mkdir ~/.config
$ cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https
```
Создаем котолог в домашней папке, создаем файл и записываем в него элементы конфигурации, затем устанавливаем её

```sh
$ mkdir projects/lab02 && cd projects/lab02
$ git init
```
Создаем папку и в ней инициализирусем git
```
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:   git branch -m <name>
Initialized empty Git repository in /home/kali/Ramil/workspace/projects/lab02/.git/
```

```sh
$ git config --global user.name ${GITHUB_USERNAME}
$ git config --global user.email ${GITHUB_EMAIL}
# check your git global settings
$ git config -e --global
```
устанавливаем конфигурацию используя заранее созданный конфиг

```sh
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
$ git pull origin master
```
Привязываем удаленный репозиторий к локальному с именем origin и совмещаем их

```sh
$ touch README.md
$ git status
$ git add README.md
$ git commit -m"added README.md"
$ git push origin master
```
Добавляем файл, смотрим изменения,добавляем изменения в индекс, комитим и отправляем на удаленный репозиторий
```
commit 9bfe7beb225791ba9ae5d265dd0936af23ec4849 (HEAD -> master, origin/master)
Author: Ramil-caramel <ramilrr2706@gmail.com>
Date:   Mon Mar 3 18:00:07 2025 +0000

    .gitignore

commit d0142e572ea33af4d25f00e237d6531ab93125dd
Author: Ramil-caramel <ramilrr2706@gmail.com>
Date:   Mon Mar 3 20:35:37 2025 +0000

    added README.md
```
```
[master (root-commit) 920c45b] added README.md
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
```
```
Username for 'https://github.com': Ramil-caramel
Password for 'https://Ramil-caramel@github.com': 
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 221 bytes | 221.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/Ramil-caramel/lab02.git
 * [new branch]      master -> master
```

Далее создаем папки, в них создаем файлы cpp, затем комитим их по той же схеме
С помощью gist пишем репорт
