## Laboratory work V
Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере GTest
```bash
$ export GITHUB_USERNAME=<имя_пользователя>
$ alias gsed=sed # for *-nix system
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab05
$ cd projects/lab05
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
```
создаем переменую окружения, псевдоним, а также копируем репозиторий и перепривязываем origin к своему новому репозиторию
```
Cloning into 'projects/lab05'...
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 22 (delta 3), reused 22 (delta 3), pack-reused 0 (from 0)
Receiving objects: 100% (22/22), done.
Resolving deltas: 100% (3/3), done.
```
```bash
$ mkdir third-party
$ git submodule add https://github.com/google/googletest third-party/gtest
$ cd third-party/gtest && git checkout release-1.8.1 && cd ../..
$ git add third-party/gtest
$ git commit -m"added gtest framework"
```
создаем папку, добавляем подмодуль, устанавливаем ему версию, возвращаясь в исходную папку, делаем комит
```
Cloning into '/home/rama/Ramil-caramel/workspace/projects/lab05/third-party/gtest'...
remote: Enumerating objects: 27954, done.
remote: Counting objects: 100% (146/146), done.
remote: Compressing objects: 100% (103/103), done.
remote: Total 27954 (delta 85), reused 44 (delta 42), pack-reused 27808 (from 5)
Receiving objects: 100% (27954/27954), 13.86 MiB | 21.02 MiB/s, done.
Resolving deltas: 100% (20696/20696), done.

[master b147161] added gtest framework
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest

```
```bash
$ gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check \${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
EOF
```
Добавляем строку в фалй Cmakelist.txt, а затем записываем в него еще данные
```bash
$ mkdir tests
$ cat > tests/test1.cpp <<EOF
#include <print.hpp>
.
.
.
```
создаем файл с тестом
```bash
$ cmake -H. -B_build -DBUILD_TESTS=ON
$ cmake --build _build
$ cmake --build _build --target test
$ _build/check
$ cmake --build _build --target test -- ARGS=--verbose
```
Включаем тесты,собираем весь проект и конкретную цель
```
-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/rama/Ramil-caramel/workspace/projects/lab05/_build

[  8%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 16%] Linking CXX static library libprint.a
[ 16%] Built target print
[ 25%] Building CXX object third-party/gtest/googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 33%] Linking CXX static library ../../../lib/libgtest.a
[ 33%] Built target gtest
[ 41%] Building CXX object third-party/gtest/googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Linking CXX static library ../../../lib/libgtest_main.a
[ 50%] Built target gtest_main
[ 58%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[ 66%] Linking CXX executable check
[ 66%] Built target check
[ 75%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 83%] Linking CXX static library ../../../lib/libgmock.a
[ 83%] Built target gmock
[ 91%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../../../lib/libgmock_main.a
[100%] Built target gmock_main

Running tests...
Test project /home/rama/Ramil-caramel/workspace/projects/lab05/_build
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

[==========] Running 1 test from 1 test suite.
[----------] Global test environment set-up.
[----------] 1 test from Print
[ RUN      ] Print.InFileStream
[       OK ] Print.InFileStream (0 ms)
[----------] 1 test from Print (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test suite ran. (0 ms total)
[  PASSED  ] 1 test.

```
```bash
$ gsed -i 's/lab04/lab05/g' README.md
$ gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml
$ gsed -i '/cmake --build _build --target install/a\
- cmake --build _build --target test -- ARGS=--verbose
' .travis.yml
$ travis lint
```
вносим изменения для коректной работы trevis(кторый не работае:(  
Далее добавляем комит и пушим его
```
Enumerating objects: 33, done.
Counting objects: 100% (33/33), done.
Delta compression using up to 12 threads
Compressing objects: 100% (22/22), done.
Writing objects: 100% (33/33), 4.30 KiB | 4.30 MiB/s, done.
Total 33 (delta 7), reused 19 (delta 3), pack-reused 0
remote: Resolving deltas: 100% (7/7), done.
To https://github.com/Ramil-caramel/lab05
 * [new branch]      master -> master

```
```bash
$ mkdir artifacts
$ sleep 20s && gnome-screenshot --file artifacts/screenshot.png
# for macOS: $ screencapture -T 20 artifacts/screenshot.png
# open https://github.com/${GITHUB_USERNAME}/lab05
```
делаем скриншот
