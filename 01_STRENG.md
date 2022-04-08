# СТРОКИ

## Обрезать начальные и конечные пробелы из строки

Это альтернатива `sed`, `awk`, `perl` и другим инструментам.
Функция находит все начальные и конечные пробелы, и удаляет их из начала и конца строки.
Вместо временной переменной - используется встроенная `:`.

**Пример функции:**

```sh
trim_string() {
    # Использование: trim_string "   example   string    "
    : "${1#"${1%%[![:space:]]*}"}"
    : "${_%"${_##*[![:space:]]}"}"
    printf '%s\n' "$_"
}
```

**Пример использования:**

```shell
$ trim_string "    Hello,  World    "
Hello,  World

$ name="   John Black  "
$ trim_string "$name"
John Black
```


## Обрезать начальные и конечные пробелы из строки, и обрезать пробелы внутри строки

Это альтернатива `sed`, `awk`, `perl` и другим инструментам.
Функция находит все пробелы, как в начале и в конце строки, так и внутри строки. И удаляет их из начала и конца строки, и обрезает внутри строки.

**Пример функции:**

```sh
# shellcheck disable=SC2086,SC2048
trim_all() {
    # Использование: trim_all "   example   string    "
    set -f
    set -- $*
    printf '%s\n' "$*"
    set +f
}
```

****Пример использования:**
:**

```shell
$ trim_all "    Hello,    World    "
Hello, World

$ name="   John   Black  is     my    name.    "
$ trim_all "$name"
John Black is my name.
```

## Использовать регулярное выражение для строки

Результат сопоставления регулярных выражений `bash` может быть использован для замены `sed` для большого количества вариантов использования.

**ВНИМАНИЕ**: это одна из немногих функций bash, зависящих от платформы.
`bash` будет использовать любой механизм регулярных выражений, установленный в системе пользователя.
Придерживайтесь функций регулярных выражений POSIX, если стремитесь к совместимости.

**ВНИМАНИЕ**: В этом примере печатается только первая совпадающая группа. При использовании нескольких групп захвата требуется некоторая модификация.

**Пример функции:**

```sh
regex() {
    # Использование: regex "string" "regex"
    [[ $1 =~ $2 ]] && printf '%s\n' "${BASH_REMATCH[1]}"
}
```

**Пример использования:**

```shell
$ # Обрезать начальные пробелы.
$ regex '    hello' '^\s*(.*)'
hello

$ # Проверить шестнадцатеричный код цвета.
$ regex "#FFFFFF" '^(#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3}))$'
#FFFFFF

$ # Проверить шестнадцатеричный цвет (invalid).
$ regex "red" '^(#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3}))$'
# no output (invalid)
```

**Пример использования в скрипте:**

```shell
is_hex_color() {
    if [[ $1 =~ ^(#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3}))$ ]]; then
        printf '%s\n' "${BASH_REMATCH[1]}"
    else
        printf '%s\n' "error: $1 is an invalid color."
        return 1
    fi
}

read -r color
is_hex_color "$color" || color="#FFFFFF"

# Do stuff.
```


## Разделить строку по разделителю

**ВНИМАНИЕ:** требуется `bash` не ниже 4+

Это альтернатива `cut`, `awk` и другим инструментам.

**Пример функции:**

```sh
split() {
   # Использование: split "string" "delimiter"
   IFS=$'\n' read -d "" -ra arr <<< "${1//$2/$'\n'}"
   printf '%s\n' "${arr[@]}"
}
```

**Пример использования:**

```shell
$ split "apples,oranges,pears,grapes" ","
apples
oranges
pears
grapes

$ split "1, 2, 3, 4, 5" ", "
1
2
3
4
5

# Многосимвольные разделители тоже работают!
$ split "hello---world---my---name---is---john" "---"
hello
world
my
name
is
john
```

## Изменить строку на нижний регистр

**ВНИМАНИЕ:** требуется `bash` не ниже 4+

**Пример функции:**

```sh
lower() {
    # Использование: lower "string"
    printf '%s\n' "${1,,}"
}
```

**Пример использования:**

```shell
$ lower "HELLO"
hello

$ lower "HeLlO"
hello

$ lower "hello"
hello
```

## Изменить строку на верхний регистр

**ВНИМАНИЕ:** требуется `bash` не ниже 4+

**Пример функции:**

```sh
upper() {
    # Использование: upper "string"
    printf '%s\n' "${1^^}"
}
```

**Пример использования:**

```shell
$ upper "hello"
HELLO

$ upper "HeLlO"
HELLO

$ upper "HELLO"
HELLO
```

## Обратный регистр строки

**ВНИМАНИЕ:** требуется `bash` не ниже 4+

**Пример функции:**

```sh
reverse_case() {
    # Использование: reverse_case "string"
    printf '%s\n' "${1~~}"
}
```

**Пример использования:**

```shell
$ reverse_case "hello"
HELLO

$ reverse_case "HeLlO"
hElLo

$ reverse_case "HELLO"
hello
```

## Вырезать кавычки из строки

**Пример функции:**

```sh
trim_quotes() {
    # Использование: trim_quotes "string"
    : "${1//\'}"
    printf '%s\n' "${_//\"}"
}
```

**Пример использования:**

```shell
$ var="'Hello', \"World\""
$ trim_quotes "$var"
Hello, World
```

## Удалить все экземпляры шаблона из строки

**Пример функции:**

```sh
strip_all() {
    # Использование: strip_all "string" "pattern"
    printf '%s\n' "${1//$2}"
}
```

**Пример использования:**

```shell
$ strip_all "The Quick Brown Fox" "[aeiou]"
Th Qck Brwn Fx

$ strip_all "The Quick Brown Fox" "[[:space:]]"
TheQuickBrownFox

$ strip_all "The Quick Brown Fox" "Quick "
The Brown Fox
```

## Удалить первое вхождение шаблона из строки

**Пример функции:**

```sh
strip() {
    # Использование: strip "string" "pattern"
    printf '%s\n' "${1/$2}"
}
```

**Пример использования:**

```shell
$ strip "The Quick Brown Fox" "[aeiou]"
Th Quick Brown Fox

$ strip "The Quick Brown Fox" "[[:space:]]"
TheQuick Brown Fox
```

## Удалить шаблон с начала строки

**Пример функции:**

```sh
lstrip() {
    # Использование: lstrip "string" "pattern"
    printf '%s\n' "${1##$2}"
}
```

**Пример использования:**

```shell
$ lstrip "The Quick Brown Fox" "The "
Quick Brown Fox
```

## Удалить шаблон с конца строки

**Пример функции:**

```sh
rstrip() {
    # Использование: rstrip "string" "pattern"
    printf '%s\n' "${1%%$2}"
}
```

**Пример использования:**

```shell
$ rstrip "The Quick Brown Fox" " Fox"
The Quick Brown
```

## Кодирование строки

**Пример функции:**

```sh
urlencode() {
    # Использование: urlencode "string"
    local LC_ALL=C
    for (( i = 0; i < ${#1}; i++ )); do
        : "${1:i:1}"
        case "$_" in
            [a-zA-Z0-9.~_-])
                printf '%s' "$_"
            ;;

            *)
                printf '%%%02X' "'$_"
            ;;
        esac
    done
    printf '\n'
}
```

**Пример использования:**

```shell
$ urlencode "https://github.com/dylanaraps/pure-bash-bible"
https%3A%2F%2Fgithub.com%2Fdylanaraps%2Fpure-bash-bible
```

## Декодирование строки

**Пример функции:**

```sh
urldecode() {
    # Использование: urldecode "string"
    : "${1//+/ }"
    printf '%b\n' "${_//%/\\x}"
}
```

**Пример использования:**

```shell
$ urldecode "https%3A%2F%2Fgithub.com%2Fdylanaraps%2Fpure-bash-bible"
https://github.com/dylanaraps/pure-bash-bible
```

## Проверить, содержит ли строка подстроку

**Использование оператора тест:**

```shell
if [[ $var == *sub_string* ]]; then
    printf '%s\n' "sub_string is in var."
fi

# Инверсия (подстрока не в строке).
if [[ $var != *sub_string* ]]; then
    printf '%s\n' "sub_string is not in var."
fi

# Это работает и для массивов!
if [[ ${arr[*]} == *sub_string* ]]; then
    printf '%s\n' "sub_string is in array."
fi
```

**Использование оператора case:**

```shell
case "$var" in
    *sub_string*)
        # Do stuff
    ;;

    *sub_string2*)
        # Do more stuff
    ;;

    *)
        # Else
    ;;
esac
```

## Проверить, начинается ли строка с подстроки

```shell
if [[ $var == sub_string* ]]; then
    printf '%s\n' "var starts with sub_string."
fi

# Инверсия (var не начинается с sub_string).
if [[ $var != sub_string* ]]; then
    printf '%s\n' "var does not start with sub_string."
fi
```

## Проверить, заканчивается ли строка подстрокой

```shell
if [[ $var == *sub_string ]]; then
    printf '%s\n' "var ends with sub_string."
fi

# Инверсия (var не заканчивается на sub_string).
if [[ $var != *sub_string ]]; then
    printf '%s\n' "var does not end with sub_string."
fi
```
