## Laboratory work 4
Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса Travis CI
```bash
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_TOKEN=<полученный_токен>
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```
создаем переменные окружения, переходим в дирректорию и активируем скрипт
```bash
$ \curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles
$ echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate
$ . scripts/activate
$ rvm autolibs disable
$ rvm install ruby-2.4.2
$ rvm use 2.4.2 --default
$ gem install travis
```
устанавливаем систему контроля версий ruby, скачиваем старую версию и устанавливаем её как основную, затем скачиваем travis
```bash
$ git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04
$ cd projects/lab04
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04
```
копируем удаленный репозиторий и перепривязваем  origin к lab04
```
Cloning into 'projects/lab04'...
remote: Enumerating objects: 18, done.
remote: Counting objects: 100% (18/18), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 18 (delta 2), reused 18 (delta 2), pack-reused 0 (from 0)
Receiving objects: 100% (18/18), done.
Resolving deltas: 100% (2/2), done.
```

```bash
$ cat > .travis.yml <<EOF
language: cpp
EOF
$ cat >> .travis.yml <<EOF

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
$ cat >> .travis.yml <<EOF

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
$ cat >> .travis.yml <<EOF

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```
создаем и записываем настройки в файл .travis.yml
```bash
$ travis login --github-token ${GITHUB_TOKEN}
$ travis lint
```
логинимся в тревис и проверяем правильность заполнения файла
```
Successfully logged in as Ramil-caramel!

Hooray, .travis.yml looks valid :)

```
```bash
$ ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md
$ git add .travis.yml
$ git add README.md
$ git commit -m"added CI"
$ git push origin master
```
Изменяем README.md, создаем и отправляем комит
```
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 12 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 576 bytes | 576.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/Ramil-caramel/lab04
 + c9165cd...f59b597 master -> master (forced update)
```
```bash
$ travis lint
$ travis accounts
$ travis sync
$ travis repos
$ travis enable
$ travis whatsup
$ travis branches
$ travis history
$ travis show
```
```
Hooray, .travis.yml looks valid :)
Ramil-caramel (Ramil-caramel): not subscribed, 18 repositories
To set up a subscription, please visit app.travis-ci.com.
synchronizing: .. done

Detected repository as Ramil-caramel/lab04, is this correct? |yes| yes
Ramil-caramel/lab04: enabled :)

```
