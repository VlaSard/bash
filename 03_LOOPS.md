# ЦИКЛЫ

## Цикл по диапазону чисел

Альтернатива `seq`.

```shell
# Цикл от 0 до 100 (без поддержки переменных).
for i in {0..100}; do
    printf '%s\n' "$i"
done
```

## Цикл по переменному диапазону чисел

Альтернатива `seq`.

```shell
# Цикл от 0-VAR.
VAR=50
for ((i=0;i<=VAR;i++)); do
    printf '%s\n' "$i"
done
```

## Цикл по массиву

```shell
arr=(apples oranges tomatoes)

# Просто элементы.
for element in "${arr[@]}"; do
    printf '%s\n' "$element"
done
```

## Цикл по массиву с индексом

```shell
arr=(apples oranges tomatoes)

# Элементы и индекс.
for i in "${!arr[@]}"; do
    printf '%s\n' "${arr[i]}"
done

# Альтернативный метод.
for ((i=0;i<${#arr[@]};i++)); do
    printf '%s\n' "${arr[i]}"
done
```

## Цикл по содержимому файла

```shell
while read -r line; do
    printf '%s\n' "$line"
done < "file"
```

## Цикл по файлам и каталогам

Не используйте `ls`.

```shell
# Жадный пример.
for file in *; do
    printf '%s\n' "$file"
done

# PNG файлы в каталоге.
for file in ~/Pictures/*.png; do
    printf '%s\n' "$file"
done

# Итерация по каталогам.
for dir in ~/Downloads/*/; do
    printf '%s\n' "$dir"
done

# Расширение скобки.
for file in /path/to/parentdir/{file1,file2,subdir/file3}; do
    printf '%s\n' "$file"
done

# Итерировать рекурсивно.
shopt -s globstar
for file in ~/Pictures/**/*; do
    printf '%s\n' "$file"
done
shopt -u globstar
```
