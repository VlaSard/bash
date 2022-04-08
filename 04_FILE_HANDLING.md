# ОБРАБОТКА ФАЙЛОВ

**ПРЕДОСТЕРЕЖЕНИЕ:** `bash` неправильно обрабатывает двоичные данные в версиях ниже `4.4`.

## Прочитать файл в строку

Альтернатива команде `cat`.

```shell
file_data="$(<"file")"
```

## Чтение файла в массив (*построчно*)

Альтернатива команде `cat`.

```shell
# bash версии ниже 4 (с отбрасыванием пустых строк).
IFS=$'\n' read -d "" -ra file_data < "file"

# bash версии ниже 4 (с сохранением пустых строк).
while read -r line; do
    file_data+=("$line")
done < "file"

# bash версии выше 4+
mapfile -t file_data < "file"
```

## Прочитать первые N строк файла

Альтернатива команде `head`.

**ВНИМАНИЕ:** требуется `bash` версии выше 4+

**Пример функции:**

```sh
head() {
    # вызов: head "n" "file"
    mapfile -tn "$1" line < "$2"
    printf '%s\n' "${line[@]}"
}
```

**Пример использования:**

```shell
$ head 2 ~/.bashrc
# Prompt
PS1='➜ '

$ head 1 ~/.bashrc
# Prompt
```

## Прочитать последние N строк файла

Альтернатива команде `tail`.

**ВНИМАНИЕ:** требуется `bash` версии выше 4+

**Пример функции:**

```sh
tail() {
    # вызов: tail "n" "file"
    mapfile -tn 0 line < "$2"
    printf '%s\n' "${line[@]: -$1}"
}
```

**Пример использования:**

```shell
$ tail 2 ~/.bashrc
# Enable tmux.
# [[ -z "$TMUX"  ]] && exec tmux

$ tail 1 ~/.bashrc
# [[ -z "$TMUX"  ]] && exec tmux
```

## Получить количество строк в файле

Альтернатива команде `wc -l`.

**Пример функции (bash 4):**

```sh
lines() {
    # вызов: lines "file"
    mapfile -tn 0 lines < "$1"
    printf '%s\n' "${#lines[@]}"
}
```

**Пример функции (bash 3):**

Этот метод использует меньше памяти, чем метод `mapfile`, и работает в `bash` 3, но медленнее для больших файлов.

```sh
lines_loop() {
    # вызов: lines_loop "file"
    count=0
    while IFS= read -r _; do
        ((count++))
    done < "$1"
    printf '%s\n' "$count"
}
```

**Пример использования:**

```shell
$ lines ~/.bashrc
48

$ lines_loop ~/.bashrc
48
```

## Подсчет файлов или каталогов в каталоге

Работает путем передачи вывода glob в функцию, а затем подсчета количества аргументов.

**Пример функции:**

```sh
count() {
    # вызов: count /path/to/dir/*
    #        count /path/to/dir/*/
    printf '%s\n' "$#"
}
```

**Пример использования:**

```shell
# подсчитать все файлы в каталоге.
$ count ~/Downloads/*
232

# подсчитать все каталоги в каталоге.
$ count ~/Downloads/*/
45

# подсчитать все файлы jpg в каталоге.
$ count ~/Pictures/*.jpg
64
```

## Создать пустой файл

Альтернатива команде `touch`.

```shell
# короткий вариант.
>file

# более длинные альтернативы:
:>file
echo -n >file
printf '' >file
```

## Извлечение данных между двумя маркерами

**Пример функции:**

```sh
extract() {
    # вызов: extract file "opening marker" "closing marker"
    while IFS=$'\n' read -r line; do
        [[ $extract && $line != "$3" ]] &&
            printf '%s\n' "$line"

        [[ $line == "$2" ]] && extract=1
        [[ $line == "$3" ]] && extract=
    done < "$1"
}
```

**Пример использования:**

```shell
# Извлечь блоки кода из файла MarkDown.
$ extract ~/projects/pure-bash/README.md '```sh' '```'
# Output here...
```
