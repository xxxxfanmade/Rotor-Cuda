# Rotor-Cuda
Это модифицированая версия [VanitySearch](https://github.com/JeanLucPons/VanitySearch/). 
Огромная благодарность [kanhavishva](https://github.com/kanhavishva) и всем разработчикам, чьи коды были использованы в Rotor-Cuda.
## Параметры:
```
Usage: Rotor-Cuda [options...]
Options:
    -v, --version          Показать версию
    -c, --check            Проверить работоспособнось программы
    -u, --uncomp           Search uncompressed addresses
    -b, --both             Search both uncompressed or compressed addresses
    -g, --gpu              Enable GPU calculation
    -i, --gpui             GPU ids: 0,1...: List of GPU(s) to use, default is 0
    -x, --gpux             GPU gridsize: g0x,g0y,g1x,g1y, ...: Specify GPU(s) kernel gridsize, default is 8*(MP number),128
    -o, --out              Выходной файл: вывод результата в ваш файл.txt, по умолчанию: Found.txt
    -m, --max              Specify maximun number of addresses found by each kernel call
    -s, --seed             Seed: Specify a seed for the base key, default is random
    -t, --thread           Сколько задействовать ядер процессора CPU Рекомендую -t 0 (не использовать CPU)
    -e, --nosse            Disable SSE hash function
    -l, --list             List cuda enabled devices
    -r, --rkey             Через сколько генерировать новый хэш, по умолчании отключено. Рекомендую -r 1000000000
    -n, --nbit             В каком битовом диапазоне генерировать хэши. Рекомендую -n 256 (-n 255) 
    -f, --file             RIPEMD160 binary hash file path
    -h, --help             Shows this page

```

```
Rotor-Cuda.exe -t 0 -g -i 0 -x 256,256 -n 120 -f address1-160-sorted.bin

Rotor v1.00

MODE         : COMPRESSED
DEVICE       : GPU
CPU THREAD   : 0
GPU IDS      : 0
GPU GRIDSIZE : 256x256
SSE          : YES
SEED         :
RKEY(Mk)     : 0
NBIT         : 120
MAX FOUND    : 65536
HASH160 FILE : address1-160-sorted.bin
OUTPUT FILE  : Found.txt

Loading      : 100 %
Loaded       : 73,446 address

Bloom at 00000233F3AFE6F0
  Version    : 2.1
  Entries    : 146892
  Error      : 0.0000010000
  Bits       : 4223905
  Bits/Elem  : 28.755175
  Bytes      : 527989 (0 MB)
  Hash funcs : 20

Start Time   : Sun Mar 28 01:39:10 2021
Base Key     : 0000000000000000000000000000000000B2E2584BCDDD8EF7382DF58863DB4B (120 bit)

GPU          : GPU #0 GeForce GTX 1650 (14x64 cores) Grid(256x128)

[00:00:08] [CPU+GPU: 494.87 Mk/s] [GPU: 494.87 Mk/s] [T: 4,026,531,840] [F: 0]
```

## Запуск рандома:
```
Rotor-Cuda.exe -t 0 -g -i 0 -x 256,256 -n 256 -r 1000000000 -f address1-160-sorted.bin
```
- Это самая быстрая программа рандом генерации!
- Скорость зависит параметров и количества адресов в базе.
- С небольшой базой ~10.000 адресов RTX2070 выдает 1200-1400 Mk/s

## Запуск рандома с отображением генерации хэшей:
```
Rotor-Cuda-display-hex.exe -t 0 -g -i 0 -x 256,256 -n 256 -r 1000000000 -f address1-160-sorted.bin
```
- В отличии от других программ Rotor-Cuda не выводит генерируемые хэши на экран. 
- Сделал экспериментальную, тестовую версию Rotor-Cuda-display-hex.exe 
- Сделана она для того что бы немного увидеть кота в мешке. 
- Из-за вывода генерируемых хэшей на экран, очень падает скорость.
- Используйте её только для тестов
- 
## Часто задаваемые вопросы:
- [Как создать базу адресов .bin ?](https://github.com/phrutis/Rotor/issues/1)
- [Гипотеза - Почему Random лучше перебора ?](https://github.com/phrutis/Rotor/issues/3)

## Donation
- BTC: bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

