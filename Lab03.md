## Laboratory work 3
Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере CMake
## Report
```bash
$ export GITHUB_USERNAME=<имя_пользователя>
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```
создаем переменную окружения, переходим в дирикторию, добавляем директорию в стек и активируем скрипт добавляющий доп путь в PATH
```
~/Ramil/workspace ~/Ramil/workspace ~/Ramil/workspace
#я делал это пару раз так как забыл об этом
```
```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
$ cd projects/lab03
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```
клонировали и переименовали репозиторий себе на компьютор, перепривизали origin к новому удаленному репозиторию
```sh
$ g++ -std=c++11 -I./include -c sources/print.cpp
$ ls print.o
$ nm print.o | grep print
$ ar rvs print.a print.o
$ file print.a
$ g++ -std=c++11 -I./include -c examples/example1.cpp
$ ls example1.o
$ g++ example1.o print.a -o example1
$ ./example1 && echo
```
компилируем файл print.cpp в объектный, проверяем создался ли он, выводим список функций связанный с print. Создаёт статическую библиотеку print.a из объектного файла print.o
Проверяем тип файла. Компилируем пример линкуем его с созданной статичной библитекой и запускаем, echo выведет ничего если все сработает нормально
```
print.o

0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000026 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E

ar: creating print.a
a - print.o

print.a: current ar archive


hello
     
```

```sh
$ g++ -std=c++11 -I./include -c examples/example2.cpp
$ nm example2.o
$ g++ example2.o print.a -o example2
$ ./example2
$ cat log.txt && echo
```
Повторяем с exemple 2
```
0000000000000000 V DW.ref.__gxx_personality_v0
                 U __gxx_personality_v0
0000000000000000 T main
                 U _Unwind_Resume
                 U _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
0000000000000000 W _ZNSt15__new_allocatorIcED1Ev
0000000000000000 W _ZNSt15__new_allocatorIcED2Ev
0000000000000000 n _ZNSt15__new_allocatorIcED5Ev
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
                 U _ZSt21ios_base_library_initv
0000000000000000 r _ZStL19piecewise_construct
                                                
```
```sh
$ rm -rf example1.o example2.o print.o
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
```
Удаляем предыдущие файлы
```sh
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```
Устанавливаем минимальную версиб,название проекта,стандарт и его обязательность,затем говорим cmake созлать статическую библиотеку print из одноименного файла и добавляем дирекуторию заголовков
```sh
$ cmake -H. -B_build
$ cmake --build _build
```
Создаются файлы cmake и происходит сборка проекта,на выходе получим библиотеку
```
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print

```
```sh
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```
Добавляем исполняемые файлы и ставим цели, то есть что с чем будет линковаться
```sh
$ cmake --build _build
$ cmake --build _build --target print
$ cmake --build _build --target example1
$ cmake --build _build --target example2
```
Выполняем сборку и всего проекта и отдельных его частей
```sh
$ ls -la _build/libprint.a
$ _build/example1 && echo
hello
$ _build/example2
$ cat log.txt && echo
hello
$ rm -rf log.txt
```
Проверяем создались ли файлы там где нужно, выполняем их, а затем удаляем 
```sh
$ git clone https://github.com/tp-labs/lab03 tmp
$ mv -f tmp/CMakeLists.txt .
$ rm -rf tmp
```
Скопировали репозиторий, файлом из него замменили наш и удалили папку скачанного репозитория
```sh
$ cat CMakeLists.txt
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
$ cmake --build _build --target install
$ tree _install
```
Читаем файл, создаем файлы сборки указывая куда нужно будет поместить готовый результат, также выводим дерево
```
_install
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
    └── libprint.a

4 directories, 4 files

```
отправляем результаты на наш репозиторий
