# ПЕРЕМЕННЫЕ

## Назначение и доступ к переменной с помощью переменной

```shell
$ hello_world="value"

# Создайте имя переменной.
$ var="world"
$ ref="hello_$var"

# Распечатайте значение имени переменной, хранящееся в 'hello_$var'.
$ printf '%s\n' "${!ref}"
value
```

Альтернативно, на `bash` 4.3+:

```shell
$ hello_world="value"
$ var="world"

# Объявить ссылку на имя переменной.
$ declare -n ref=hello_$var

$ printf '%s\n' "$ref"
value
```

## Имя переменной на основе другой переменной

```shell
$ var="world"
$ declare "hello_$var=value"
$ printf '%s\n' "$hello_world"
value
```
