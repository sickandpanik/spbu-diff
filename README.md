# Курс основ программирования на МКН СПбГУ

## Проект 1: утилита diff
[Постановка задачи](./TASK.md)

### Использование
`java -jar diff.jar [ОПЦИИ] ФАЙЛ1 ФАЙЛ2` (запускать из корня проекта)

Консольная утилита diff сравнивает два файла ФАЙЛ1 и ФАЙЛ2 и выводит разницу между ними в формате
последовательности из общих, удалённых и добавленных строк. Желаемый формат вывода задаётся ОПЦИЯМИ.

### Входные данные
На вход подаются два пути к файлам (абсолютные или относительные). Максимальный размер файла — 10 МБайт,
максимальное количество строк — 10000.

### Обработка файлов
diff находит наибольшую общую подпоследовательность двух файлов с использованием простого алгоритма, работающего
с асимптотикой O(NM), где N и M — количество строк в первом и втором файле соответственно.

### Выходные данные
Вывод осуществляется в консоль (стандартный вывод). Используйте `... diff.jar ... > out.txt`, чтобы перенаправить вывод в файл `out.txt`.

Если поданные на вход файлы одинаковые, то программа ничего не выводит. В противном случае, если не было передано ОПЦИЙ,
вывод осуществляется в «нормальном» формате. Различные режимы вывода не сочетаются между собой. Вызов различных форматов вывода описан
ниже.

### «Объединённый» формат вывода
ОПЦИЯ: `-u[CONTEXT_LINES] --unified=[CONTEXT_LINES]`

«Объединённый» формат вывода (используется, например, в Github).

Сначала выводятся имена сравниваемых файлов и время последнего изменения этих файлов. Затем выводятся блоки изменений с контекстом CONTEXT_LINES (по умолчанию 3) строк около каждого
блока (по умолчанию три строки). Если блоки с контекстом пересекаются, то они объединяются в один блок.

Каждый блок предваряется заголовком в формате `@@ -s1,l1 +s2,l2 @@` (на выводе без кавычек), где s1 — начало блока относительно
первого файла, l1 — количество строк в блоке, содержащихся в первом файле. Аналогично s2 и l2 определяются для второго
файла.

### Нормальный формат вывода
ОПЦИЯ: `-n --normal`

«Нормальный» формат вывода (используется по умолчанию).

Изменённые блоки выводятся без контекста вокруг; удаленные строки отмечаются знаком
< в начале строки, добавленные — знаком >.

Начало каждого блока предваряет описание изменений в формате `fot`, где `o` — символ, обозначающий характер операции (`a` — добавление, `d` — удаление, `c` — изменение), `f` описывает область в
первом файле, откуда были удалены или куда были бы вставлены строчки из второго файла, если бы их вставили в первый (например, `1,3` означает что были изменены или удалены строчки с 1 по 3).
Аналогично, `t` описывает область во втором файле, куда были добавлены или где были бы строчки из первого файла,
если бы их не удалили.

### Простой формат вывода
ОПЦИЯ: `--plain`

Простой формат вывода.

Выводит объединение обоих файлов. Удалённые строки показываются минусом в начале строки, добавленные — плюсом.

### Получение помощи
Используйте ОПЦИЮ `--help`, чтобы получить краткую справку и завершить программу.

### Тестирование и результаты тестов
В папке `src/test/kotlin/files/small_files` находятся небольшие файлы, на которых можно проверить программу. Можно запустить
bash-скрипт `test_on_small_files.sh` в корневой директории, который выведет все файлы и покажет на них результат работы diff
(как в одну сторону, так и в другую).

Там же есть папка 
`large_files`, на которых я протестировал насколько пригодна моя программа в качестве рабочего инструмента нахождения разницы
между файлами с помощью утилиты `patch`. 

Результаты: так как разработка велась на Windows, в файлах окончания строк в стиле Windows (`\r\n`). `patch` не умеет работать
с этим. Если изменить окончания строк на UNIX-стиль (`\n`), то патчи применяются на 6/10 — хотя бы один блок применяется с ошибкой,
особенно с этим проблемы в «объединённом» режиме вывода.