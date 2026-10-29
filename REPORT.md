# Отчет по лабораторно-практической работе: «Работа с ветками в Git»

**Выполнил:** Студент Курбанали (Kurbanali)  
**GitHub:** [Lachowski262](https://github.com/Lachowski262)  
**Репозиторий проекта:** `my-branching-repo`  
**Удаленный репозиторий (Remote):** `https://github.com/Lachowski262/my-branching-repo.git`  
**Среда выполнения:** Visual Studio Code (интегрированный терминал PowerShell, модуль Source Control, встроенный редактор конфликтов Merge Editor)

---

## 1. Цель работы

Научиться практической работе с ветками в системе контроля версий Git с использованием редактора Visual Studio Code:
- Создавать новые ветки и безопасно переключаться между ними;
- Выполнять различные типы слияний (Fast-Forward и трехстороннее слияние);
- Моделировать ситуации конфликта версий и разрешать их с помощью инструментов VS Code;
- Удалять локальные ветки после завершения работы;
- Связывать локальный репозиторий с удаленным сервером (GitHub) с помощью команды `git remote`.

---

## 2. Пошаговое выполнение работы в Visual Studio Code

### Шаг 1. Подготовка репозитория и привязка к Remote

Я открыл Visual Studio Code и через встроенный терминал инициализировал новый локальный репозиторий `my-branching-repo`.

```bash
# Инициализация репозитория
git init my-branching-repo
cd my-branching-repo

# Настройка моих данных пользователя
git config user.name "Kurbanali"
git config user.email "hamidullaevv.k@gmail.com"

# Создание файла README.md и первый коммит
echo "# My Branching Repo" > README.md
git add README.md
git commit -m "Initial commit"
```

**Вывод терминала:**
```text
Initialized empty Git repository in C:/Users/Kurban/.gemini/antigravity/scratch/my-branching-repo/.git/
[master (root-commit) e177da4] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

**Связывание локального репозитория с удаленным (Remote):**
В соответствии с заданием, я привязал созданный репозиторий к своему удаленному аккаунту GitHub:

```bash
git remote add origin https://github.com/Lachowski262/my-branching-repo.git
git remote -v
```

**Вывод терминала:**
```text
origin  https://github.com/Lachowski262/my-branching-repo.git (fetch)
origin  https://github.com/Lachowski262/my-branching-repo.git (push)
```

---

### Шаг 2. Создание и работа с ветками (`feature-a` и `feature-b`)

В интерфейсе VS Code переключение и создание веток доступно как в строке состояния (внизу слева), так и через встроенный терминал.

1. **Создание ветки `feature-a` и внесение изменений:**
   ```bash
   git checkout -b feature-a
   echo "This is feature A" > feature-a.txt
   git add feature-a.txt
   git commit -m "Added feature-a.txt"
   ```
   *Вывод терминала:*
   ```text
   Switched to a new branch 'feature-a'
   [feature-a 6f3668e] Added feature-a.txt
    1 file changed, 1 insertion(+)
    create mode 100644 feature-a.txt
   ```

2. **Возврат в основную ветку `master`:**
   ```bash
   git checkout master
   ```
   *Вывод терминала:*
   ```text
   Switched to branch 'master'
   ```

3. **Создание ветки `feature-b` и внесение изменений:**
   ```bash
   git checkout -b feature-b
   echo "This is feature B" > feature-b.txt
   git add feature-b.txt
   git commit -m "Added feature-b.txt"
   ```
   *Вывод терминала:*
   ```text
   Switched to a new branch 'feature-b'
   [feature-b d9bce56] Added feature-b.txt
    1 file changed, 1 insertion(+)
    create mode 100644 feature-b.txt
   ```

---

### Шаг 3. Слияние веток

1. **Слияние ветки `feature-a` в ветку `master`:**
   Я вернулся на ветку `master` и выполнил команду слияния:
   ```bash
   git checkout master
   git merge feature-a
   ```
   *Вывод терминала:*
   ```text
   Switched to branch 'master'
   Updating e177da4..6f3668e
   Fast-forward
    feature-a.txt | 1 +
    1 file changed, 1 insertion(+)
    create mode 100644 feature-a.txt
   ```
   > **Примечание:** Слияние выполнено в режиме **Fast-forward**, так как ветка `master` не содержала коммитов после точки ветвления `feature-a`, указатель ветки просто переместился вперед.

2. **Просмотр истории коммитов:**
   ```bash
   git log --oneline --graph
   ```
   *Вывод терминала:*
   ```text
   * 6f3668e Added feature-a.txt
   * e177da4 Initial commit
   ```

3. **Слияние ветки `feature-b` в ветку `master`:**
   ```bash
   git merge feature-b
   ```
   *Вывод терминала:*
   ```text
   Merge made by the 'ort' strategy.
    feature-b.txt | 1 +
    1 file changed, 1 insertion(+)
    create mode 100644 feature-b.txt
   ```
   > **Примечание:** Здесь произошло истинное трехстороннее слияние с формированием автоматического коммита слияния (`Merge branch 'feature-b'`), так как обе ветки содержали независимые коммиты.

---

### Шаг 4. Моделирование и разрешение конфликта слияния в VS Code

1. **Создание конфликтной ситуации:**
   Я намеренно изменил один и тот же файл `feature-a.txt` в ветке `feature-a` и в ветке `master`.

   *В ветке `feature-a`:*
   ```bash
   git checkout feature-a
   echo "Conflict line from feature-a" >> feature-a.txt
   git add feature-a.txt
   git commit -m "Updated feature-a.txt"
   ```
   *Вывод:*
   ```text
   Switched to branch 'feature-a'
   [feature-a 3a2ba65] Updated feature-a.txt
    1 file changed, 1 insertion(+)
   ```

   *В ветке `master`:*
   ```bash
   git checkout master
   echo "Conflict line from master" >> feature-a.txt
   git add feature-a.txt
   git commit -m "Updated feature-a.txt in master"
   ```
   *Вывод:*
   ```text
   Switched to branch 'master'
   [master 0c41cb4] Updated feature-a.txt in master
    1 file changed, 1 insertion(+)
   ```

2. **Попытка слияния (возникновение конфликта):**
   ```bash
   git merge feature-a
   ```
   *Вывод терминала:*
   ```text
   Auto-merging feature-a.txt
   CONFLICT (content): Merge conflict in feature-a.txt
   Automatic merge failed; fix conflicts and then commit the result.
   ```

3. **Отображение конфликта в Visual Studio Code:**
   При открытии файла `feature-a.txt` редактор VS Code подсвечивает конфликтующие блоки и предлагает интерактивные действия (CodeLens):
   - `Accept Current Change` (принять текущие изменения из ветки `master`)
   - `Accept Incoming Change` (принять входящие изменения из ветки `feature-a`)
   - `Accept Both Changes` (сохранить обе версии изменений)
   - `Compare Changes` (сравнить изменения в режиме diff)
   - Кнопку **Resolve in Merge Editor** (3-панельный редактор слияния в VS Code).

   *Вид файла с маркерами конфликта:*
   ```text
   This is feature A
   <<<<<<< HEAD
   Conflict line from master
   =======
   Conflict line from feature-a
   >>>>>>> feature-a
   ```

4. **Разрешение конфликта:**
   Я выбрал объединение правок (сохранение обеих строк) и удалил маркеры конфликта.
   Итоговое содержимое файла `feature-a.txt`:
   ```text
   This is feature A
   Conflict line from master
   Conflict line from feature-a
   ```

5. **Фиксация разрешения конфликта:**
   ```bash
   git add feature-a.txt
   git commit -m "Resolved conflict in feature-a.txt"
   ```
   *Вывод терминала:*
   ```text
   [master 1ec83bc] Resolved conflict in feature-a.txt
   ```

---

### Шаг 5. Удаление локальных веток

После успешного слияния всех изменений в основную ветку я удалил вспомогательные ветки:

```bash
git branch -d feature-a
git branch -d feature-b
```

**Вывод терминала:**
```text
Deleted branch feature-a (was 3a2ba65).
Deleted branch feature-b (was d9bce56).
```

Проверка списка веток:
```bash
git branch
```
*Вывод:*
```text
* master
```

---

### Шаг 6. Итоговая проверка состояния репозитория

```bash
git status
git log --oneline --graph --all
```

**Вывод `git status`:**
```text
On branch master
nothing to commit, working tree clean
```

**Вывод `git log --oneline --graph --all`:**
```text
*   1ec83bc Resolved conflict in feature-a.txt
|\  
| * 3a2ba65 Updated feature-a.txt
* | 0c41cb4 Updated feature-a.txt in master
* |   93e83ca Merge branch 'feature-b'
|\ \  
| |/  
|/|   
| * d9bce56 Added feature-b.txt
* | 6f3668e Added feature-a.txt
|/  
* e177da4 Initial commit
```

**Команда для отправки в удаленный репозиторий на GitHub:**
```bash
git push -u origin master
```

---

## 3. Описание возникших проблем и их решение

1. **Проблема:** Возникновение `CONFLICT (content): Merge conflict in feature-a.txt` при слиянии веток с модификацией одного участка кода.  
   **Решение:** Использованы встроенные визуальные инструменты Visual Studio Code (Merge Editor). Выбран вариант сохранения согласованного содержимого (`Accept Both Changes`), маркеры конфликта удалены, файл повторно добавлен в индекс (`git add`) и зафиксирован коммит слияния (`git commit`).

2. **Проблема:** Предупреждения `warning: in the working copy, LF will be replaced by CRLF` в Windows-окружении.  
   **Решение:** Это штатное поведение Git на платформе Windows при включенной директиве `core.autocrlf=true`. Параметр автоматически преобразует окончания строк в CRLF при извлечении и в LF при коммите в репозиторий, что гарантирует совместимость.

3. **Проблема:** Необходимость автоматического и удобного контроля веток без риска потерять незакоммиченные правки.  
   **Решение:** В VS Code панель **Source Control** (Ctrl+Shift+G) и нижняя панель статуса наглядно отображают текущую ветку, наличие синхронизированных коммитов и привязку `origin`, что предотвращает случайные коммиты в не ту ветку.

---

## 4. Заключение

В процессе выполнения лабораторной работы в среде Visual Studio Code я:
1. Освоил полный цикл работы с ветвлением в Git (`git checkout -b`, `git branch`, `git merge`, `git branch -d`).
2. На практике исследовал различия между слиянием методом Fast-Forward и трехсторонним слиянием с созданием merge-коммита.
3. Научился эффективно использовать визуальный функционал VS Code для локализации и разрешения конфликтов слияния.
4. Успешно связал локальный репозиторий с удаленным репозиторием GitHub (`git remote add origin`).
