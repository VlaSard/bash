# МАССИВЫ

## Перевернуть массив

Включение extdebug позволяет получить доступ к массиву BASH_ARGV, в котором хранятся аргументы текущей функции в обратном порядке.

**ВНИМАНИЕ**: требуется `shop -s compat44` в `bash` версии не ниже 5.0+.

**Пример функции:**

```sh
reverse_array() {
    # Использования: reverse_array "array"
    shopt -s extdebug
    f()(printf '%s\n' "${BASH_ARGV[@]}"); f "$@"
    shopt -u extdebug
}
```

**Пример использования:**

```shell
$ reverse_array 1 2 3 4 5
5
4
3
2
1

$ arr=(red blue green)
$ reverse_array "${arr[@]}"
green
blue
red
```

## Удалить повторяющиеся элементы массива

Создайте временный ассоциативный массив. При установке значений ассоциативного массива и возникновении дублирующего назначения, bash перезаписывает ключ. Это позволяет нам эффективно удалять дубликаты массивов.

**ВНИМАНИЕ:** требуется `bash` версии не ниже 4+

**ПРЕДУПРЕЖДЕНИЕ.** Порядок списка может измениться.

**Пример функции:**

```sh
remove_array_dups() {
    # Использования: remove_array_dups "array"
    declare -A tmp_array

    for i in "$@"; do
        [[ $i ]] && IFS=" " tmp_array["${i:- }"]=1
    done

    printf '%s\n' "${!tmp_array[@]}"
}
```

**Пример использования:**

```shell
$ remove_array_dups 1 1 2 2 3 3 3 3 3 4 4 4 4 4 5 5 5 5 5 5
1
2
3
4
5

$ arr=(red red green blue blue)
$ remove_array_dups "${arr[@]}"
red
green
blue
```

## Случайный элемент массива

**Пример функции:**

```sh
random_array_element() {
    # Использования: random_array_element "array"
    local arr=("$@")
    printf '%s\n' "${arr[RANDOM % $#]}"
}
```

**Пример использования:**

```shell
$ array=(red green blue yellow brown)
$ random_array_element "${array[@]}"
yellow

# Multiple arguments can also be passed.
$ random_array_element 1 2 3 4 5 6 7
3
```

## Цикл по массиву

Каждый раз, когда вызывается `printf`, печатается следующий элемент массива. Когда печать достигает последнего элемента массива, она снова начинается с первого элемента.

```sh
arr=(a b c d)

cycle() {
    printf '%s ' "${arr[${i:=0}]}"
    ((i=i>=${#arr[@]}-1?0:++i))
}
```


## Переключение между двумя значениями

Это работает так же, как и выше, это просто другой вариант использования.

```sh
arr=(true false)

cycle() {
    printf '%s ' "${arr[${i:=0}]}"
    ((i=i>=${#arr[@]}-1?0:++i))
}
```
