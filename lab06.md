##Laboratory work VI
Данная лабораторная работа посвещена изучению средств пакетирования на примере CPack
```bash
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_EMAIL=<адрес_почтового_ящика>
$ alias edit=<nano|vi|vim|subl>
$ alias gsed=sed # for *-nix system
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
$ git clone https://github.com/${GITHUB_USERNAME}/lab05 projects/lab06
$ cd projects/lab06
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION\
  \${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_TWEAK 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
$ git diff
```
Создаем переменные окружения и псевдонимы, копируем репозиторий и переходим в него. 
В файл CMakeLsit.md дописываем несколько строк устанавливающие версию для нашего проекта
```
Cloning into 'projects/lab06'...
remote: Enumerating objects: 33, done.
remote: Counting objects: 100% (33/33), done.
remote: Compressing objects: 100% (18/18), done.
remote: Total 33 (delta 7), reused 33 (delta 7), pack-reused 0 (from 0)
Receiving objects: 100% (33/33), 4.30 KiB | 733.00 KiB/s, done.
Resolving deltas: 100% (7/7), done.


diff --git a/CMakeLists.txt b/CMakeLists.txt
index c4d9df1..489bb7d 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -7,6 +7,13 @@ option(BUILD_EXAMPLES "Build examples" OFF)
 option(BUILD_TESTS "Build tests" OFF)
 
 project(print)
+set(PRINT_VERSION_MAJOR 0)
+set(PRINT_VERSION_MINOR 1)
+set(PRINT_VERSION_PATCH 0)
+set(PRINT_VERSION_TWEAK 0)
+set(PRINT_VERSION
+  ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})
+set(PRINT_VERSION_STRING "v${PRINT_VERSION}")

```
```bash
$ touch DESCRIPTION && edit DESCRIPTION
$ touch ChangeLog.md
$ export DATE="`LANG=en_US date +'%a %b %d %Y'`"
$ cat > ChangeLog.md <<EOF
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF
$ cat > CPackConfig.cmake <<EOF
include(InstallRequiredSystemLibraries)
EOF
```
Создаем файлы описания и логов, яоздаем переменную времени, файл изменений логов и в файл cmake для CPak, в котором команда на установку требуемых библиотек
Далее мы продолжаем вносить в файл конфигов CPack изменения в которых указываем наши данные, версии проекта, подключаем файл с описанием, лицензией README.md
Затем задаем настройки для создания пакетов RPM , тое сть название лицензия, группы пакетов и версии

Затем в Cmake добавляем наш файл с конфигами
```bash
$ gsed -i 's/lab05/lab06/g' README.md
```
заменяем все вхождения старого репозитория на новый
```bash
$ git add .
$ git commit -m"added cpack config"
$ git tag v0.1.0.0
$ git push origin master --tags
$ cmake -H. -B_build
$ cmake --build _build
$ cd _build
$ cpack -G "TGZ"
$ cd ..
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
$ cmake --build _build --target package
```
комитим и собираем проект
```
Enumerating objects: 39, done.
Counting objects: 100% (39/39), done.
Delta compression using up to 12 threads
Compressing objects: 100% (23/23), done.
Writing objects: 100% (39/39), 5.18 KiB | 5.18 MiB/s, done.
Total 39 (delta 9), reused 31 (delta 7), pack-reused 0
remote: Resolving deltas: 100% (9/9), done.
To https://github.com/Ramil-caramel/lab06
 * [new branch]      master -> master
 * [new tag]         v0.1.0.0 -> v0.1.0.0

CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/rama/Ramil-caramel/workspace/projects/lab06/_build

[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print

CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/rama/Ramil-caramel/workspace/projects/lab06/_build/print-0.1.0.0-Linux.tar.gz generated.

[100%] Built target print
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/rama/Ramil-caramel/workspace/projects/lab06/_build/print-0.1.0.0-Linux.tar.gz generated.

```
```bash
$ mkdir artifacts
$ mv _build/*.tar.gz artifacts
$ tree artifacts
```
создаем папку перемещаем в неё все архивы и выводим дерево
```
artifacts
└── print-0.1.0.0-Linux.tar.gz

1 directory, 1 file

```
