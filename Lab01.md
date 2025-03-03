## Laboratory work I

Данная лабораторная работа посвещена изучению утилит для разработки проектов
## Report
```bash
$ export GITHUB_USERNAME=<username>
$ export GIST_TOKEN=<saved_token>
$ alias edit=<nano|vi|vim|subl>
```
Устанавливаем переменные окружения и псевдоним ждя команды edit
```sh
$ mkdir -p ${GITHUB_USERNAME}/workspace
$ cd ${GITHUB_USERNAME}/workspace
$ pwd
```
Создаем директорию, переходим в неё и выводим её адрес
```
/home/kali/Ramil/workspace
```

```sh
$ cd ..
$ pwd
```
Переходим на одну дерикторию выше и выводим ее адрес
```
/home/kali/Ramil
```

```sh
$ mkdir -p workspace/tasks/
$ mkdir -p workspace/projects/
$ mkdir -p workspace/reports/
$ cd workspace
```
Создаем несколько папок и переходим в папку workspace
Будет создана структура директорий:

    workspace/
    ├── tasks/
    ├── projects/
    └── reports/


```sh
# Debian
$ wget https://nodejs.org/dist/v6.11.5/node-v6.11.5-linux-x64.tar.xz
$ tar -xf node-v6.11.5-linux-x64.tar.xz
$ rm -rf node-v6.11.5-linux-x64.tar.xz
$ mv node-v6.11.5-linux-x64 node
```
После выполнения всех команд в текущей директории останется папка node, содержащая распакованную версию Node.js

```sh
$ ls node/bin
$ echo ${PATH}
```
Выводим содержимое папки node/bin и значение переменной окружения PATH 
```sh
$ export PATH=${PATH}:`pwd`/node/bin
$ echo ${PATH}
```
Добавляем в path путь до этой папки чтобы затем мы могли отовсюда испольховать утилиту Node.js и выводим обновленное значение PATH
```sh
$ mkdir scripts
$ cat > scripts/activate<<EOF
export PATH=\${PATH}:`pwd`/node/bin
EOF
$ source scripts/activate
```
создаем папку, далее команда создает файл scripts/activate и записывает в него следующие строки до EOF. 
cat > файл означает «записать ввод в файл» (в данном случае текст будет записан в scripts/activate).
source выполняет команды из файла scripts/activate

```sh
$ gem install gist
```
Устанавливаем Gist

```sh
$ (umask 0077 && echo ${GIST_TOKEN} > ~/.gist)
```
Устанавливаем права записи и записываем в скрытый файл наш токен, так как без него мы не сможем ничего отправлять

```sh
$ export LAB_NUMBER=01
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```
Создаем папку с копией лабораторной работы, ызаимодействуем с файлом с помощью текстового редактора, затем отправляем на github 
