# ИНФОРМАЦИЯ О ТЕРМИНАЛЕ

## Получить размер терминала в строках и столбцах (*из скрипта*)

Это удобно при написании скриптов на чистом bash, когда нельзя вызвать `stty`/`tput`.

**Пример функции:**

```sh
get_term_size() {
    # вызов: get_term_size
    # (:;:) это микрозадержка, чтобы обеспечить
    # немедленный экспорт переменных.
    shopt -s checkwinsize; (:;:)
    printf '%s\n' "$LINES $COLUMNS"
}
```

**Пример использования:**

```shell
# вывод: LINES COLUMNS
$ get_term_size
15 55
```

## Получить размер терминала в пикселях

**ВНИМАНИЕ**: это не работает в некоторых эмуляторах терминала.

**Пример функции:**

```sh
get_window_size() {
    # вызов: get_window_size
    printf '%b' "${TMUX:+\\ePtmux;\\e}\\e[14t${TMUX:+\\e\\\\}"
    IFS=';t' read -d t -t 0.05 -sra term_size
    printf '%s\n' "${term_size[1]}x${term_size[2]}"
}
```

**Пример использования:**

```shell
# вывод: WIDTHxHEIGHT
$ get_window_size
1200x800

# вывод (fail):
$ get_window_size
x
```

## Получить текущую позицию курсора

Это полезно при создании TUI в чистом bash.

**Пример функции:**

```sh
get_cursor_pos() {
    # вызов: get_cursor_pos
    IFS='[;' read -p $'\e[6n' -d R -rs _ y x _
    printf '%s\n' "$x $y"
}
```

**Пример использования:**

```shell
# вывод: X Y
$ get_cursor_pos
1 8
```

<!-- CHAPTER END -->

