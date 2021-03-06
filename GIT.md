# GIT

[toc]

## [Книга по GIT](https://git-scm.com/book/ru/v2)

### Состояния файлов в рабочей копии

В первую очередь все файлы делятся на `untracked` (файл который не был добавлен для отслеживания командой `git add`) и `tracked` - отслеживаемые.

Если какие-то файлы нужны только для разработки, и не должны быть зафиксированы (`git commit`) необходимо внести их в файл `.gitignore`  , если файл уже был добавлен для отслеживания, то необходимо удалить соответствующие данные из репозитория (`git rm`)

#### Tracked (отслеживаемые) файлы

Данная категория файлов может находиться в одном из 3-х состояний:

1. Unmodified (неизмененное)
2. Modified (измененное)
3. Staged (отслеживаемое)

1 - Файл точно такой же как и в репозитории.

2 - Сюда переходит любой файл после `git commit` 

НО! Modified != Tracked, те нам еще нужно перевести файл в состояние 3 с помощью ` git add` , вот теперь мы можем пользоваться `git commit`  

Команда `git add -i`  запускает интерактивную сессию , показывая изменения в файлах.

### Сброс изменений

`git reset /path/to/file` переводит состояние `staged` в `modified` 

`git checkout -- path/to/file` переводит файл из состояния `modified` в `unmodified` - те по сути сбрасывает изменения

### Состояние файлов в GIT

Staged файлы (После команды `git add` ) хранятся в Staged area, откуда могут быть зафиксированы в репозитории командрй `git commit` . А файлы скаченый из репозитория, хранятся в Working directory.

### Ветвление

[Онлайн-обучалка](https://learngitbranching.js.org)

Команда ` git status` показывает информацию о ветке в которой мы находимся и другие моменты (modified/tracked файлы) и тд

Все коммиты представляют из себя `LinkedList`

Ветка - это ссылка на опреджеленный кормит.

`git checkout <название ветки>` позволяет выбрать ветку

`git branch` показывает все ветки , а текущую отмечает символом `*` 

HEAD - это указатель **текущей ветки** 

(`git log --oneline --decorate --graph`)

Те для переключения по веткам используется назначение HEAD  в нужную точку (коммит).

### Отправка изменений на сервер (PUSH)

`git push origin new-feature`

Отправляет staged в удаленный репозиторий с именем `origin` в ветку `new-feature`

Но набирать такую команду долго, проще один раз сделать ветку отслеживаемой (upstream branch)

`git push --set-upstream origin new-feature`

Выполняет 2 действия :

1. Устанавливает отслеживание
2. Отправляет изменеия

теперь можно отправлять изменения `git push`

Таким образом локальная ветка связана с веткой на удаленном репозитории.

Информация об отслеживаемой ветке лежит в `.git/config`

### Получение изменений (PULL)

`git pull` - позволяет получить изменения из удаленной ветки  и слить их

Под капотом у `git pull` две команды: 

1. `git fetch`
2. `git merge`

Хорошим тоном считается отсутствие коммитов слияния (мердж коммиты). Чтобы история была "прямой" используют `git pull --rebase`

Подробнее о `rebase` [тут](https://git-scm.com/book/ru/v1/Ветвление-в-Git-Перемещение)

Это основные команды, необходимые для ежедневной работы, остальное в книге (в заголовке)

### Git Hooks

