##Laboratory work IX
Данная лабораторная работа посвещена изучению процесса создания артефактов на примере Github Release
```bash
`
$ export GITHUB_TOKEN=<полученный_токен>
$ export GITHUB_USERNAME=<имя_пользователя>
$ export PACKAGE_MANAGER=<пакетный менеджер>
$ export GPG_PACKAGE_NAME=<gpg2|gpg>

# for *-nix system
$ $PACKAGE_MANAGER install xclip
$ alias gsed=sed
$ alias pbcopy='xclip -selection clipboard'
$ alias pbpaste='xclip -selection clipboard -o'

$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
$ go get github.com/aktau/github-release
```
устанавливаем переменные окружения, скачиваем xclip и создаем псевдонимы, переходим в папку и скачиваем пакет

```bash
$ $PACKAGE_MANAGER install ${GPG_PACKAGE_NAME}
$ gpg --list-secret-keys --keyid-format LONG
$ gpg --full-generate-key
$ gpg --list-secret-keys --keyid-format LONG
$ gpg -K ${GITHUB_USERNAME}
$ GPG_KEY_ID=$(gpg --list-secret-keys --keyid-format LONG | grep ssb | tail -1 | awk '{print $2}' | awk -F'/' '{print $2}')
$ GPG_SEC_KEY_ID=$(gpg --list-secret-keys --keyid-format LONG | grep sec | tail -1 | awk '{print $2}' | awk -F'/' '{print $2}')
$ gpg --armor --export ${GPG_KEY_ID} | pbcopy
$ pbpaste
$ open https://github.com/settings/keys
$ git config user.signingkey ${GPG_SEC_KEY_ID}
$ git config gpg.program gpg
```
устанавливаем gpg, проверяем какие ключи существют, создаем новый, снова проверяем, ищем id ключей, и добавляем их к своему гитхаб, чтобы наши комиты подписывались им
```bash
$ test -r ~/.bash_profile && echo 'export GPG_TTY=$(tty)' >> ~/.bash_profile
$ echo 'export GPG_TTY=$(tty)' >> ~/.profile
```
проверяем bash profile и добавляем переменную которая указывает gpg каким терминалом мы пользуемся
```bash
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
$ cmake --build _build --target package
```
```
.
.МНОГО ТЕКСТА
.МНОГО ТЕКСТА
.
.
-- [hunter] GTEST_ROOT: /home/rama/.hunter/_Base/23f1b5a/fb15dbb/bf2be25/Install (ver.: 1.11.0)
-- Found GTest: /home/rama/.hunter/_Base/23f1b5a/fb15dbb/bf2be25/Install/lib/cmake/GTest/GTestConfig.cmake (found version "1.11.0")  
-- Configuring done (4.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/rama/Ramil-caramel/workspace/projects/lab09/_build


[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
[ 75%] Building CXX object CMakeFiles/demo.dir/demo/main.cpp.o
[100%] Linking CXX executable demo
[100%] Built target demo
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/rama/Ramil-caramel/workspace/projects/lab09/_build/print-0.1.0.0-Linux.tar.gz generated.

```
```bash
$ git tag -s v0.1.0.0
$ git tag -v v0.1.0.0
$ git show v0.1.0.0
$ git push origin master --tags
```
Создали тег и отправили его
```
object e5585b7fe29160632216d5e417f26a9b6d15a1c9
type commit
tag v0.1.0.0
tagger Ramil-caramel <ramilrr2706@gmail.com> 1742755998 +0300

Release version 0.1.0.0

```
```bash
$ github-release --version
$ github-release info -u ${GITHUB_USERNAME} -r lab09
$ github-release release \
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "libprint" \
    --description "my first release"
```
выводим информацию о репозитории,делаем релиз
```
tags:
- v0.1.0.0 (commit: https://api.github.com/repos/Ramil-caramel/lab09/commits/e5585b7fe29160632216d5e417f26a9b6d15a1c9)
releases:

```
```bash
$ github-release info -u ${GITHUB_USERNAME} -r lab09
$ wget https://github.com/${GITHUB_USERNAME}/lab09/releases/download/v0.1.0.0/${PACKAGE_FILENAME}
$ tar -ztf ${PACKAGE_FILENAME}
```
выводим информацию, скачиваем наш релиз и проверяем его
```
tags:
- v0.1.0.0 (commit: https://api.github.com/repos/Ramil-caramel/lab09/commits/e5585b7fe29160632216d5e417f26a9b6d15a1c9)
releases:
- v0.1.0.0, name: 'libprint', description: 'my first release', id: 207710144, tagged: 23/03/2025 at 18:53, published: 23/03/2025 at 19:00, draft: ✗, prerelease: ✗
  - artifact: print-0.1.0.0-Linux.tar.gz, downloads: 0, state: uploaded, type: application/octet-stream, size: 5.9 kB, id: 240086290

print-0.1.0.0-Linux/cmake/
print-0.1.0.0-Linux/cmake/print-config.cmake
print-0.1.0.0-Linux/cmake/print-config-noconfig.cmake
print-0.1.0.0-Linux/bin/
print-0.1.0.0-Linux/bin/demo
print-0.1.0.0-Linux/lib/
print-0.1.0.0-Linux/lib/libprint.a
print-0.1.0.0-Linux/include/
print-0.1.0.0-Linux/include/print.hpp

```
