# lab04
## Теория

Мы **не используем** Travis CI! Лабы старые просто. Туториал игнорьте.

**Инструкции** представляют собой файлы формата `.yml`.

Размещать их необходимо в директории `.github/workflows`.

В ней можно создать несколько файлов yml для разных задач.

Инструкции запускаются на серверах гитхаба.

**Триггер** — условие, при котором запускается процесс.

`${{github.workspace}}` — переменная, в которой хранится путь до рабочей директории на удалённом сервере гитхаба. Все пути следует писать относительно неё.

**Важно:** yml очень придирчив к табуляции. Никаких табов, только пробелы.

**Структура** файла yml:

```yaml
name: CMake
# Задаёт имя процесса

on:
# Задаёт список триггеров
 push:
  branches: [main]
 pull_request:
  branches: [main]
# В данном случае процесс запустится при push и pull-request в ветку main

jobs: 
# Задаёт список задач
 build_Linux:
 # Название задачи

  runs-on: ubuntu-latest
  # Система, на которой будет запущена задача
  # В данном случае это будет последняя версия Ubuntu

  steps:
  # Шаги задачи
  - uses: actions/checkout@v3
  # Включает поддержку GitHub Actions, вроде как

  - name: Configure App
    # Имя шага
    run: cmake ${{github.workspace}}/project -B ${{github.workspace}}/project/build
    # Команда

  - name: Build App
    run: cmake --build ${{github.workspace}}/app/build
```

При пушах и пулл-реквестах в ветку `main` сервис GitHub Actions прогонит на своих серверах заданные нами команды по сборке проекта и поделится с нами результатами (успешно или нет).

В разделе **Actions** на странице репозитория можно подробно ознакомиться с тем, как всё прошло.

___

## Laboratory work IV

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```sh
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

> Это написано для Travis CI, так что **игнорим Tutorial**

```sh
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_TOKEN=<полученный_токен>
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```

```sh
$ \curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles
$ echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate
$ . scripts/activate
$ rvm autolibs disable
$ rvm install ruby-2.4.2
$ rvm use 2.4.2 --default
$ gem install travis
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04
$ cd projects/lab04
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04
```

```sh
$ cat > .travis.yml <<EOF
language: cpp
EOF
```

```sh
$ cat >> .travis.yml <<EOF

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
```

```sh
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

```sh
$ travis login --github-token ${GITHUB_TOKEN}
```

```sh
$ travis lint
```

```sh
$ ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md
```

```sh
$ git add .travis.yml
$ git add README.md
$ git commit -m"added CI"
$ git push origin master
```

```sh
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

## Report

```sh
$ popd
$ export LAB_NUMBER=04
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Homework

Вы продолжаете проходить стажировку в "Formatter Inc." (см [подробности](https://github.com/tp-labs/lab03#Homework)).

В прошлый раз ваше задание заключалось в настройке автоматизированной системы **CMake**.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в [прошлый раз](https://github.com/tp-labs/lab03#Homework). Настройте сборочные процедуры на различных платформах:
* используйте [TravisCI](https://travis-ci.com/) для сборки на операционной системе **Linux** с использованием компиляторов **gcc** и **clang**;
* используйте [AppVeyor](https://www.appveyor.com/) для сборки на операционной системе **Windows**.

> - используйте GitHub Actions и для того, и для того)

> Клонируем репозиторий третьей лабы:
> ```sh
> $ git clone <ссылка на репу третьей лабы>
> $ cd <имя репы третьей лабы>
> $ git remote remove origin
> $ git remote add origin <ссылка на репу четвёртой лабы>
> ```
>
> Создаём директорию `.github/workflows` в директории лабы
>
> В ней создаём файл `CI.yml`:
>
> ```yaml
> name: CMake
> 
> on:
>  push:
>   branches: [main]
>  pull_request:
>   branches: [main]
> 
> jobs: 
>  build_Linux:
> 
>   runs-on: ubuntu-latest
> 
>   steps:
>   - uses: actions/checkout@v3
> 
>   - name: Configure Solver
>     run: cmake ${{github.workspace}}/solver_application/ -B ${{github.workspace}}/solver_application/build
> 
>   - name: Build Solver
>     run: cmake --build ${{github.workspace}}/solver_application/build
> 
>   - name: Configure HelloWorld
>     run: cmake ${{github.workspace}}/hello_world_application/ -B ${{github.workspace}}/hello_world_application/build
> 
>   - name: Build HelloWorld
>     run: cmake --build ${{github.workspace}}/hello_world_application/build
> 
>  build_Windows:
> 
>   runs-on: windows-latest
> 
>   steps:
>   - uses: actions/checkout@v3
> 
>   - name: Configure Solver
>     run: cmake ${{github.workspace}}/solver_application/ -B ${{github.workspace}}/solver_application/build
> 
>   - name: Build Solver
>     run: cmake --build ${{github.workspace}}/solver_application/build
> 
>   - name: Configure HelloWorld
>     run: cmake ${{github.workspace}}/hello_world_application/ -B ${{github.workspace}}/hello_world_application/build
> 
>   - name: Build HelloWorld
>     run: cmake --build ${{github.workspace}}/hello_world_application/build
> ```
>
> Добавляем файл в проект и пушим на гитхаб:
> ```sh
> $ git add .github
> $ git commit -m "added CI.yml"
> $ git push origin main
> ```
> Заходим на гитхаб, во вкладку **Actions**, и проверяем, что процесс выполнен успешно.
