## Laboratory work IV

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```ShellSession
$ open https://travis-ci.org
```

## Tasks

- [ ] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
- [ ] 2. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [ ] 3. Ознакомиться со ссылками учебного материала
- [ ] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
- [ ] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
- [ ] 6. Получить фрагмент вставки значка сервиса **Travis CI** в формате **Markdown**
- [ ] 7. Выполнить инструкцию учебного материала
- [ ] 8. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя> # присваиваем переменной  окружения GITHUB_USERNAME значение
$ export GITHUB_TOKEN=<полученный_токен> # присваиваем переменной окружения GITHUB_TOKEN значение
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace # переходим в директорию
$ pushd . # сохраняем имя текущего каталога для команды popd
$ source scripts/activate # выполняем команды из файла
```

```ShellSession
$ \curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles
$ echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate # записываем текст в файл
$ . scripts/activate 
$ rvm autolibs disable # отключаем autolibs
$ rvm install ruby-2.4.2 # устанавливаем ruby-2.4.2
$ rvm use 2.4.2 --default # устанавливаем версию 2.4.2 по умолчанию
$ gem install travis # устанавливаем travis
```
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04 # получаем копию репозитория
$ cd projects/lab04 #переходим в каталог
$ git remote remove origin # удаляем с ветки origin доступ к предыдущему репозиторию
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04 # добавляем доступ к новому репозиторию
```

```ShellSession
$ cat > .travis.yml <<EOF # создание файла .travis.yml. Добавление языка С++.
language: cpp
EOF
```

```ShellSession
$ cat >> .travis.yml <<EOF # добавление script для автоматизированной сборки CMake.

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
```

```ShellSession
$ cat >> .travis.yml <<EOF # добавление списка пакетов, которые необходимо установить перед началом работы.

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```

```ShellSession
$ travis login --github-token ${GITHUB_TOKEN} # выполняем вход в свой аккаунт Travis
```

```ShellSession
$ travis lint # проверяем .travis.yml на наличие ошибок
```

```ShellSession
$ ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md # вставляем в README ссылку на значок о прохождении тестов
```

```ShellSession
$ git add .travis.yml # добавляем в отслеживаемые файл .travis.yml
$ git add README.md # добавляем в отслеживаемые файл README.md
$ git commit -m"added CI" # выполняем коммит
$ git push origin master # отправляем изменения на удалённый репозиторий
```

```ShellSession
$ travis lint # проверяем .travis.yml на наличие ошибок
$ travis accounts # выводим действующие аккаунты 
$ travis sync # синхронизируем данные с GitHub
$ travis repos # выводим репозитории и их статус на Travis
$ travis enable # запускаем проект
$ travis whatsup # вывод списка результатов последних сборок
$ travis branches # выводим последний тест с ветки master
$ travis history # выводим историю сборок
$ travis show # выводим результаты теста
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=04
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

Вы продолжаете проходить стажировку в "Formatter Inc." (см [подробности](https://github.com/tp-labs/lab03#Homework)).

В прошлый раз ваше задание заключалось в настройке автоматизированной системы **CMake**.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в [прошлый раз](https://github.com/tp-labs/lab03#Homework). Настройте сборочные процедуры на различных платформах:
* используйте [TravisCI](https://travis-ci.com/) для сборки на операционной системе **Linux** с использованием компиляторов **gcc** и **clang**;
* используйте [AppVeyor](https://www.appveyor.com/) для сборки на операционной системе **Windows**.

## Links

- [Travis Client](https://github.com/travis-ci/travis.rb)
- [AppVeyour](https://www.appveyor.com/)
- [GitLab CI](https://about.gitlab.com/gitlab-ci/)

```
Copyright (c) 2015-2019 The ISC Authors
```
