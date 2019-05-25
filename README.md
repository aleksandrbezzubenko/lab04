## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [ ] 2. Ознакомиться со ссылками учебного материала
- [ ] 3. Выполнить инструкцию учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**


## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd . # показывает путь до текущей папки
$ source scripts/activate
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03 # скопировали файлы из удалённого репозитория в папку projects/lab03
$ cd projects/lab03 # зашли в папку projects/lab03
$ git remote remove origin # удаляем ссылку
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git # добавляем ссылку
```

```ShellSession
$ g++ -std=c++11 -I./include -c sources/print.cpp # компиляция с помощью g++
$ ls print.o # проверяем существование
$ nm print.o | grep print 
$ ar rvs print.a print.o # aрхивируем r-Replace v-Provide verbose output s-Write an object-file index into the archive
$ file print.a # oпределяем тип файла
$ g++ -std=c++11 -I./include -c examples/example1.cpp # компиляция с помощью g++
$ ls example1.o # проверяем существование
$ g++ example1.o print.a -o example1 # компиляция с помощью g++
$ ./example1 && echo № запускаем скомпилированный файл и переход на следующую строку
```

```ShellSession
$ g++ -std=c++11 -I./include -c examples/example2.cpp # компиляция с помощью g++
$ nm example2.o # выводим на экран бинарное содержимое объектного файла
$ g++ example2.o print.a -o example2 # компиляция с помощью g++
$ ./example2 # запускаем программу
$ cat log.txt && echo # выводим на экран содержимое файла в единый поток
```

```ShellSession
$ rm -rf example1.o example2.o print.o # удаляем файлы объектного кода
$ rm -rf print.a # удаляем архив
$ rm -rf example1 example2 # удаляем скомпилированные файлы
$ rm -rf log.txt # удаляем файл txt
```

```ShellSession
$ cat > CMakeLists.txt <<EOF # Запись в файл CMakeLists.txt (добавляем cmake_minimum_required VERSION)
cmake_minimum_required(VERSION 3.4)
project(print)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF # Запись в файл CMakeLists.txt (добавляем cmake standart)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF # Запись в файл CMakeLists.txt (add library)
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF # Запись в файл CMakeLists.txt
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

```ShellSession
$ cmake -H. -B_build # # подготовливам процесс построения проекта, проверяем работоспособность и записываем результат в файл
-- The C compiler identification is GNU 7.3.0
-- The CXX compiler identification is GNU 7.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/asus/TIMP/workspace/projects/lab03/_build

$ cmake --build _build # # создаем сгенерированное ранее двоичное дерево проекта
Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF # Запись в файл CMakeLists.txt (добавляем add executable)

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF # Запись в файл CMakeLists.txt (добавляем target link libraries)

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

```ShellSession
$ cmake --build _build # запускаем сборку в текущем каталоге
$ cmake --build _build --target print # запускаем сборку с целью print вместо цели по умолчанию
$ cmake --build _build --target example1 # запускаем сборку с целью example1 вместо цели по умолчанию
$ cmake --build _build --target example2 # запускаем сборку с целью example2 вместо цели по умолчанию
```

```ShellSession
$ ls -la _build/libprint.a # выводим содержимое файла с расширением .a (статический файл библиотеки)
$ _build/example1 && echo # выполняем сборку и выводим на экран
hello
$ _build/example2 # выполняем сборку 
$ cat log.txt && echo # выводим на экран содержимое файла в единый поток
hello
$ rm -rf log.txt # удаляем файл
```

```ShellSession
$ git clone https://github.com/tp-labs/lab03 tmp # клонируем репозиторий
$ mv -f tmp/CMakeLists.txt . # перемещаем файл в текущую директорию
$ rm -rf tmp # удаляем клонированную ранее директорию
```

```ShellSession
$ cat CMakeLists.txt #выводим содержимое
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install # подготовливаеи процесс построения проекта и создаем имя префиксной команды
$ cmake --build _build --target install # запускаем сборку с целью install
$ tree _install # создание дерева
locales-launch: Data of ru_RU locale not found, generating, please wait...
_install
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   ├── print.hpp
│   └── print.hpp.gch
└── lib
    └── libprint.a

3 directories, 5 files
```

```ShellSession
$ git add CMakeLists.txt # добавление файла CMakeLists.txt для отслеживания
$ git commit -m"added CMakeLists.txt" # добавление коммита "added CMakeLists.txt"
$ git push origin master # загрузка измениний на удалённый репозиторий в ветку master
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

```
Copyright (c) 2015-2019 The ISC Authors
```
