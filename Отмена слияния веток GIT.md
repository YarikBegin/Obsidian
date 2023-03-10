Задача срочная, но простая. Нужно отменить последнее слияние ветки `bugfix/header` с `main`.

Командой `git log` вы ищете хеш последнего коммита:
```bash
git log 

commit 3f0d8dad682e05a37591bd1df4735ef209857304 (HEAD -> main) ## Необходимый коммит
Author: students-yandex <students-yandex@yandex.com>
Date:   Wed Mar 4 03:51:12 2020 +0300

Merge branch bugfix/header

```
Теперь вызываете команду git revert с опцией `-m 1` и найденным хешем коммита, который нужно отменить.

Опция -m со значением больше `0` говорит о том, какой из родителей — «основная ветка» и будет сохранён. Это важно, потому что коммит слияния получит двух родителей: «вершину ветки» `main` и сливаемой `bugfix/header`. Обычно же у коммитов один родитель.

![[Untitled_1603112926.png]]

_У коммита слияния два родителя: коммит в ветке main и последний коммит в bugfix/header_

Указав для опции `-m` значение `1`, вы сохраните всё содержимое в main и отмените изменения, внесённые веткой `bugfix/header`:
```bash
git revert -m 1 3f0d8da

[main 28547e5] Revert "Merge branch bugfix/header"

## Сразу воспользуйтесь git log, чтобы увидеть последние коммиты

git log

commit 28547e5f46f998f7ad536ae7b9117df1b9f9f6cf (HEAD -> main) ## Хеш "возвращённого" коммита
Author: students-yandex <students-yandex@yandex.com>
Date:   Wed Mar 4 04:00:10 2020 +0300

Revert "Merge branch bugfix/header'"

This reverts commit 3f0d8dad682e05a37591bd1df4735ef209857304, reversing ## Хеш отменённого коммита
changes made to 2bb5af63a96e649089dfb61d09b3c2f96908fdba. ## Из какого коммита вносились изменения
```

Команда `git revert` создала новый коммит. Он отменил изменения, внесённые коммитом слияния ветки `bugfix/header` в `main`.

![[Untitled_1_1603113057.png]]

_Команда git revert отменяет изменения, внесённые коммитом слияния веток_

Можно выдохнуть — отменились все изменения, которые пришли вместе с мержем `bugfix/header` с `main`. Можете спокойно продолжить работу в ветке `bugfix/header`. Досадно, что при новой попытке мержа ветки в `main` «Гит» сообщит вам: «Всё ок, мержить нечего».

```bash
git merge bugfix/header
Already up to date.
```

Если вы закоммитите что-нибудь в ветке `bugfix/header`, то в `main` попадут только последние изменения при слиянии.

![[Untitled_1603464841.png]]

_Только последующие коммиты в bugfix/header попадут в main_

## Слияние после отмены

Но и отмену можно отменить. Самый простой способ — создать новую ветку из той, слияние с которой было отменено. В нашем случае — из ветки `bugfix/header`.

После этого в новой ветке нужно сделать коммит, перейти в `main` и выполнить слияние:
```bash
git branch ## Посмотрим список веток и в какой ветке мы сейчас находимся
* bugfix/header
main

git checkout -b bugfix/merge-branches

## После внесения изменений в код

git add -A && git commit -m "Сделать слияние с main" # Коммитим в ветке bugfix/merge-branches

git checkout main ## Переходим в ветку main

git merge bugfix/merge-branches
```

Команда `git revert` не только отменяет коммиты слияния — она может откатить изменения, внесённые любым коммитом. Для этого нужно вызвать `git revert` без опции `-m`.

Опция `-m` не нужна, потому что обычно у коммитов один родитель: не нужно указывать, какая из веток основная.