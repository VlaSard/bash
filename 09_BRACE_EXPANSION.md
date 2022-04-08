# РАСШИРЕНИЕ СКОБОК

## Диапазоны

```shell
# Синтаксис: {<START>..<END>}

# Печать чисел типа int 1-100.
echo {1..100}

# Печать чисел типа float.
echo 1.{1..9}

# Печать символов a-z.
echo {a..z}
echo {A..Z}

# Вложение.
echo {A..C}{0..9}
# A0 A1 A2 A3 A4 A5 A6 A7 A8 A9 B0 B1 B2 B3 B4 B5 B6 B7 B8 B9 C0 C1 C2 C3 C4 C5 C6 C7 C8 C9

# Печатать числа с дополненными нулями.
# ВНИМАНИЕ: требуется bash версии не ниже 4+
echo {01..100}

# Изменить сумму приращения.
# Синтаксис: {<START>..<END>..<INCREMENT>}
# ВНИМАНИЕ: требуется bash версии не ниже 4+
echo {1..10..2} # Increment by 2.
```

## Списки строк

```shell
echo {apples,oranges,pears,grapes}

# Пример использования:
# Удалите папки Movies, Music и ISOS из ~/Downloads/.
rm -rf ~/Downloads/{Movies,Music,ISOS}
```

<!-- CHAPTER END -->

