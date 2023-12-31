# git_notes
## Рабочие заметки по git

### Начальная настройка работы с git

Настройка имени пользователя и почты (глобально)

```
git config --global user.name "Имя_пользователя"
git config --global user.email "адрес@почты.з"
```

Настройка имени пользователя и почты (локально для репозитория)

```
git config --local user.name "Имя_пользователя"
git config --local user.email "адрес@почты.з"
```

Просмотр текущих параметров настройки
```
git config --list
```

### Начало работы с репозиторием

Инициализация нового репозитория в какой-либо рабочей папке:

```
cd <рабочая_папка>
git init
```


Копирование репозитория на локальную машину с удаленного репозитория github:
```
git clone git@github.com:имя_пользователя/имя_репозитория.git
```

### Основные команды работы с репозиторием

Проверка статуса текущего репозитория:
```
git status
```


Добавление в индекс отдельного файла (его изменений):
```
git add <имя_файла>
``` 

Добавление в индекс всех изменений текущего каталога:
```
git add --all
```
или
```
git add .
```

Интерактивный режим добавления файлов в индекс:
```
git add -i
```

Чтоб навсегда исключить добавление некоторых файлов в индекс по командам `add .` или `add --all` необходимо добавить эти файлы в файла .gitignore  
**.gitignore** - файл исключений, которые не попадают в индексирование git

Просмотреть список файлов, которые игнорируются:
```
git status --ignored
```

Фиксирование добавленных в индекс изменений в репозитории:
```
git commit -m "Осмысленное сообщение об изменениях."
```

Изменение/правка последнего коммита в репозитории с изменением сообщения:
```
git commit --amend -m "Осмысленное сообщение об изменениях."
```

Изменение последнего коммита, но сообщение оставить пержним:
```
git commit --amend --no-edit 
```

Просмотр истории изменений:
```
git log
```

Просмотр истории изменений в сокращенном варианте (хэш тоже сокращенный)
```
git log --oneline
```

Просмотр истории изменений с отображением изменений (diff):
```
git log -p
```

### Статусы файлов репозитория

- untracked - неотслеживаемый файл, который не добавлен в индекс
- tracked - отслеживаемый файл (добавлен в индекс)
    - modified - измененный файл (изменения не добавлены в индекс)
    - staged - изменения файла добавлены в индекс (но еще не закомиччены)

**Схема изменения статусов файла репозитория**

```mermaid
graph LR;
  untracked -- "git add" --> staged;
  staged    -- "git commit -m <сообщение>"     --> tracked/comitted;
  tracked/comitted    -- "модификация файла"     --> modified;
  modified -- "git add" --> staged;
  staged -- "git restore --staged"     --> untracked;
  staged -- "git restore --staged" --> modified;

``` 

### Отмена выполненных изменений

Убрать файл из индекса (перевести в статус untracked/modified):
```
git restore --staged <имя_файла>
```

Отменить историю изменений (откатить рабочую копию на заданный коммит):
```
git reset --hard <hash коммита>
```
**Все! изменения, сделанные после <hash коммита> будут безвовзратно утрачены!**

Откатить изменения в рабочей копии файла до состояния, которое было сохранено по команде в git add или git commit
```
git restore <имя_файла>
```

### Анализ изменений

Сравнение модифицированных файлов с их закоммиченными версиями
```
git diff
```

Сравнение зафиксированных (staged) файлов с их закоммиченными версиями
```
git diff --staged
```

Сравнение двух разных коммитов
```
git diff <hash коммита 1> <hash коммита 2>
```


### Работа с удаленным репозиторием

Синхронизация локального и удаленного репозиториев
```
git remote add origin https://github.com/<имя_пользователя>/<наименование_репозитория>.git
```

Просмотреть подробную информацию о связи локального и удаленного репозиториев
```
git remote -v
```

Удалить привязку локального репозитория к удаленному
```
git remote rm origin
```

Первоначальная отправка всех коммитов из локального репозитория в удаленный
```
git push -u origin main
```

Отправка новой ветки <имя_ветки> из локального репозитория в удаленный
```
git push -u origin <имя_ветки>
```

Отправка текущих изменений из локального репозитория в удаленный репозиторий
```
git push
```

Забрать все изменения из удаленного репозитория и применить их в локальном
```
git pull
```


### Работа с ветками

Просмотреть все ветки проекта (в локальном репозитории)
```
git branch
```

Просмотреть все ветки проекта (в локальном и удаленном репозитории)
```
git branch -a
```

Создать новую ветку
```
git branch <имя_ветки>
```

Переключиться на ветку <имя_ветки>
```
git checkout <имя_ветки>
```

Создать новую ветку <имя_ветки> и сразу переключиться на неё
```
git checkout -b <имя_ветки>
```

Удалить ветку (если все изменения в ней уже слиты в другую ветку)
```
git branch -d <имя_ветки>
```

Удалить ветку (без контроля, слиты ли изменения в main/master)
```
git branch -D <имя_ветки>
```

Перенести все изменения (слить) из ветки <имя_ветки> в текущую ветку
```
git merge <имя_ветки>
```

Перенести все изменения (слить) из ветки <имя_ветки> в текущую ветку
```
# --no-edit отключает ввод сообщения для merge-коммита
# --no-ff отключает fast-forward слияние веток
git merge --no-edit --no-ff
```

*Сравнение веток*

Сравнение двух разных веток
```
git diff <имя_ветки1> <имя_ветки2>
```




