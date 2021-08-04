# Rotor-Cuda v1.01
![alt text](https://github.com/phrutis/Rotor/blob/main/Others/1.jpg "Rotor-Cuda")
 Это модифицированая версия [VanitySearch](https://github.com/JeanLucPons/VanitySearch/). 
 Огромная благодарность [kanhavishva](https://github.com/kanhavishva) и всем разработчикам, чьи коды были использованы в Rotor-Cuda.

## Примичания:  
- [Как создать базу адресов .bin ?](https://github.com/phrutis/Rotor/issues/1) 
- [Гипотеза - Почему Random лучше перебора](https://github.com/phrutis/Rotor/issues/3)
- Для работы программы нужно преобразовать Legacy адреса (которые начинаются на 1) в бинарные хэши160 RIPEMD160. 
- Для преобразования используйте программу [b58dec](https://github.com/phrutis/Rotor/blob/main/Others/b58dec.exe) ```Команда: b58dec.exe 1.txt 2.bin```
- Важно выполнить сортировку файла 2.bin Иначе Bloom фильтр поиска не будет работать должным образом. 
- Что бы отсортировать 2.bin используйте программу [RMD160-Sort](https://github.com/phrutis/Rotor/blob/main/Others/RMD160-Sort.exe) ```Команда: RMD160-Sort.exe 2.bin addresse160-Sort.bin```
- Минимальное количество хешей160 в addresse160-Sort.bin должно быть не менее 1000!
- Запуск 2 GPUs команда: ```Rotor-Cuda.exe -t 0 --gpu --gpux 256,256,256,256 --gpui 0,1 -n 256 -f base.bin -r 1000000000```
- Перед запуском программы сделайте окно шире, что бы информация не цеплялась за края.
- Rotor-Cuda.exe (GPU) рандомно создает несколько сотен миллионов стартовых точек, делит их на потоки, начинает перебирать. 
- Рекомендую установить ```-r 100000000000``` когда на счетчике будет T:100000000000 стартовые точки обновятся. 
- Rotor-Cuda-no-display-hex.exe без вывода рандомной генерации хешей в окно. Выглядит компактней, + небольшой прирост скорости.
## Для любителей рандома:
- На GPU ```-r 1``` - каждый хеш новый, в этом режиме скорость упадёт в ~10 раз. Будут создаваться сотни миллионов стартовых хешей. 
- В зависимости от размера Grid, чем он выше тем больше рандомных хешей создаётся. Далее хеши делятся на потоки, группируются и передаются в задачу на обработку. 
- Обработка происходит быстро, по этому с такой настройкой GPU будет ждать задачу и простаивать. Так работает CUDA. 
- Лучше использовать [Rotor3.exe](https://github.com/phrutis/Rotor) работающий на OpenCL в режиме полный рандом. Скорость меньше, за то без простоев.  
- На CPU ```-r 1``` Полный рандом! Каждый хеш новый в выбраном диапазоне -n 256 
- Для рандом генерации на CPU сделан Rotor-CPU.exe с выводом генерации новых хеше на экран. Скорость обновления данных 10ms вместо 1 сек.     
- Не используйте одновременно GPU + CPU в этом случае снизится скорость GPU. Лучше запустить две копии программы одну на CPU, вторую на GPU.
- Не используйте все ядра CPU (оставьте хоть одно), ищите только безхозные, старые адреса [без движения](https://bitinfocharts.com/ru/top-100-dormant_9y-bitcoin-addresses.html) 
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
    -r, --rkey             Через сколько рандомно генерировать новые стартовые точки. Рекомендую -r 100000000000
    -n, --nbit             В каком битовом диапазоне генерировать хэши. Рекомендую -n 256 (-n 255,254) 
    -f, --file             RIPEMD160 binary hash file path
    -h, --help             Shows this page

``` 
## Пример работы Rotor-Cuda.exe
```

C:\Users\user>Rotor-Cuda.exe -t 0 -g -i 0 -x 288,512 -f 10.bin -r 100000000000 -n 256

Rotor-Cuda v1.01 (04.08.2021)

MODE         : COMPRESSED
DEVICE       : GPU
CPU THREAD   : 0
GPU IDS      : 0
GPU GRIDSIZE : 288x512
SSE          : YES
SEED         :
RKEY(Mk)     : 100000000000
NBIT         : 256
MAX FOUND    : 65536
HASH160 FILE : 10.bin
OUTPUT FILE  : Found.txt

Loading      : 100 %
Loaded       : 90,693 address

Bloom at 000002805676CF20
  Version    : 2.1
  Entries    : 181386
  Error      : 0.0000010000
  Bits       : 5215786
  Bits/Elem  : 28.755175
  Bytes      : 651974 (0 MB)
  Hash funcs : 20

Start Time   : Tue Aug  3 22:09:05 2021
Base Key     : Reloads random start points every 100000000000
GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(288x512)

  Project Rotor-CUDA         :   https://github.com/phrutis/Rotor-Cuda
  Donate BitCoin             :   bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

 [B1F626663233C2F8EB256B060834980BB4A1CE8991798D26D1B849FEB8F10889] [00:03:28] [CPU+GPU: 1169.63 Mk/s] [GPU: 1169.63 Mk/s] [T: 202,031,235,072] [F: 0]
```
## Пример работы Rotor-CPU.exe (FULL RANDOM)
```
C:\Users\user>Rotor-Cuda.exe -t 12 -f 10.bin -r 1 -n 256

Rotor-Cuda v1.01 For CPUs Only! (04.08.2021)

MODE         : COMPRESSED
DEVICE       : CPU
CPU THREAD   : 12
GPU IDS      : 0
GPU GRIDSIZE : -1x128
SSE          : YES
SEED         :
RKEY         : 1
NBIT         : 256
MAX FOUND    : 65536
HASH160 FILE : 10.bin
OUTPUT FILE  : Found.txt

Loading      : 100 %
Loaded       : 90,693 address

Bloom at 000001A60FA2D110
  Version    : 2.1
  Entries    : 181386
  Error      : 0.0000010000
  Bits       : 5215786
  Bits/Elem  : 28.755175
  Bytes      : 651974 (0 MB)
  Hash funcs : 20

Start Time   : Wed Aug  4 20:54:35 2021
Base Key     : Reloads random start points every 1
  Project Rotor-CUDA         :   https://github.com/phrutis/Rotor-Cuda
  Donate BitCoin             :   bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

 [510D739D4717646356CAC73CF8624FF165D0FF5AA89DB7A5DEE0454D9B730417] [00:12:37] [CPU+GPU: 12.65 Mk/s] [GPU: 0.00 Mk/s] [T: 10,016,710,656] [F: 0]
 ```

## Donation
- BTC: bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

