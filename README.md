# Rotor-Cuda v1.01
![alt text](https://github.com/phrutis/Rotor/blob/main/Others/1.jpg "Rotor-Cuda")
 Это модифицированая версия [VanitySearch](https://github.com/JeanLucPons/VanitySearch/). 
 Огромная благодарность [kanhavishva](https://github.com/kanhavishva) и всем разработчикам, чьи коды были использованы в Rotor-Cuda.

## Примечания:  
- [Как создать базу адресов .bin](https://github.com/phrutis/Rotor/issues/1) 
- [Гипотеза - Почему Random лучше перебора](https://github.com/phrutis/Rotor/issues/3)
- Для работы программы нужно преобразовать Legacy адреса (которые начинаются на 1) в бинарные хэши160 RIPEMD160. 
- Для преобразования используйте программу [b58dec](https://github.com/phrutis/Rotor/blob/main/Others/b58dec.exe) ```Команда: b58dec.exe 1.txt 2.bin```
- Важно выполнить сортировку файла 2.bin Иначе Bloom фильтр поиска не будет работать должным образом. 
- Что бы отсортировать 2.bin используйте программу [RMD160-Sort](https://github.com/phrutis/Rotor/blob/main/Others/RMD160-Sort.exe) ```Команда: RMD160-Sort.exe 2.bin addresse160-Sort.bin```
- Минимальное количество хешей160 в addresse160-Sort.bin должно быть не менее 1000!
- Запуск Multi GPUs команда: ```Rotor-Cuda.exe -t 0 --gpu --gpux 256,256,256,256 --gpui 0,1 -n 256 -f base.bin -r 1000000000```
- Перед запуском программы сделайте окно шире, что бы информация не цеплялась за края.
- Rotor-Cuda.exe (GPU) рандомно создает несколько сотен миллионов стартовых точек, делит их на потоки, начинает перебирать. 
- Рекомендую установить ```-r 100000000000``` когда на счетчике будет T:100000000000 стартовые хеши обновятся. 
- Битовый диапозон округляется -n 256 или -n 253 (будет рандомить в пространстве 252-256 бит, + смещаться) Для поиска пазлов -n 64 не подходит, используйте [PubHunt](https://github.com/phrutis/PubHunt)
- Rotor-Cuda-no-display-hex.exe без вывода рандомной генерации хешей в окно. Выглядит компактней, + небольшой прирост скорости.
## Для любителей рандома Rotor-Cuda-R1:
- На GPU ```-r 1``` - каждый хеш новый, в этом режиме скорость упадёт в ~5 раз. Будут создаваться сотни миллионов хешей. 
- В зависимости от размера Grid, чем он выше, тем больше рандомных хешей создаётся. Далее хеши собираются в группы до определенного колличества, делятся на потоки и передаются в задачу на обработку. 
- Обработка происходит быстро, по этому с такой настройкой GPU будет ждать задачу и простаивать. Так работает CUDA.
- Лучше использовать [Rotor3.exe](https://github.com/phrutis/Rotor) работающий на OpenCL в режиме полный рандом. Скорость меньше, за то без простоев.  
- На CPU ```-r 1``` Полный рандом! Каждый хеш новый в выбраном диапазоне -n 256     
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
## Rotor-Cuda-R1.exe (FULL RANDOM)
### Пример работы на CPU 
```
C:\Users\user>Rotor-Cuda-R1.exe -t 6 -f 10.bin -r 1 -n 256

Rotor-Cuda v1.01 (07.08.2021)

MODE         : COMPRESSED
DEVICE       : CPU
CPU THREAD   : 6
GPU IDS      : 0
GPU GRIDSIZE : -1x128
SSE          : YES
RKEY         : 1
NBIT         : 256
MAX FOUND    : 65536
HASH160 FILE : 10.bin
OUTPUT FILE  : Found.txt

Loading      : 100 %
Loaded       : 90,693 address

Bloom at 00000255A4A6E150
  Version    : 2.1
  Entries    : 181386
  Error      : 0.0000010000
  Bits       : 5215786
  Bits/Elem  : 28.755175
  Bytes      : 651974 (0 MB)
  Hash funcs : 20

Start Time   : Sat Aug  7 01:16:18 2021
MODE         : FULL RANDOM

 [50853F9488A259839ABE332FFB61C2A9D475D5638657CC995AF17AA6FB8A8A4B] [00:04:14] [F: 0] [T: 3,961,087,232] [CPU+GPU: 14.48 Mk/s]
```
### Пример работы на GPU
```
C:\Users\user>Rotor-Cuda-R1.exe -t 0 -g -i 0 -x 288,256 -f 01.bin -r 1 -n 256

Rotor-Cuda v1.01 (07.08.2021)

MODE         : COMPRESSED
DEVICE       : GPU
CPU THREAD   : 0
GPU IDS      : 0
GPU GRIDSIZE : 288x256
SSE          : YES
RKEY         : 1
NBIT         : 256
MAX FOUND    : 65536
HASH160 FILE : 01.bin
OUTPUT FILE  : Found.txt

Loading      : 100 %
Loaded       : 1,326,779 address

Bloom at 0000021D8A2DC9B0
  Version    : 2.1
  Entries    : 2653558
  Error      : 0.0000010000
  Bits       : 76303525
  Bits/Elem  : 28.755175
  Bytes      : 9537941 (9 MB)
  Hash funcs : 20

Start Time   : Sat Aug  7 01:23:31 2021
MODE         : FULL RANDOM

GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(288x256)
  Project Rotor-CUDA         :   https://github.com/phrutis/Rotor-Cuda
  Donate BitCoin             :   bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

 [A704560033FAABFBF31753693424BD5713EF9796B916DA45591901C186B513B6] [00:01:17] [F: 0] [T: 10,871,635,968] [CPU+GPU: 0.00 Mk/s]
 ```
### Пример работы на GPU+CPU (скорость GPU упала)
```
C:\Users\user>Rotor-Cuda-R1.exe -t 6 -g -i 0 -x 288,256 -f 01.bin -r 1 -n 256

Rotor-Cuda v1.01 (07.08.2021)

MODE         : COMPRESSED
DEVICE       : CPU & GPU
CPU THREAD   : 6
GPU IDS      : 0
GPU GRIDSIZE : 288x256
SSE          : YES
RKEY         : 1
NBIT         : 256
MAX FOUND    : 65536
HASH160 FILE : 01.bin
OUTPUT FILE  : Found.txt

Loading      : 100 %
Loaded       : 1,326,779 address

Bloom at 0000016D429CC900
  Version    : 2.1
  Entries    : 2653558
  Error      : 0.0000010000
  Bits       : 76303525
  Bits/Elem  : 28.755175
  Bytes      : 9537941 (9 MB)
  Hash funcs : 20

Start Time   : Sat Aug  7 01:29:54 2021
MODE         : FULL RANDOM

 [F91F0B3E48EFC63D89615C904B19FFC5769A03776A898EC0C6BF26D7599A2ABE]GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(288x256)
  Project Rotor-CUDA         :   https://github.com/phrutis/Rotor-Cuda
  Donate BitCoin             :   bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

 [6859F3AC3C2E1C3E47EA730F1B41ED4C84AB7631E181F27C023345467F080490] [00:01:19] [F: 0] [T: 6,249,609,216] [CPU+GPU: 8.42 Mk/s]  ]
```
### Пример работы на GPU Standart
```
C:\Users\user>Rotor-Cuda-R1.exe -t 0 -g -i 0 -x 288,256 -f 01.bin -r 50000000000 -n 256

Rotor-Cuda v1.01 (07.08.2021)

MODE         : COMPRESSED
DEVICE       : GPU
CPU THREAD   : 0
GPU IDS      : 0
GPU GRIDSIZE : 288x256
SSE          : YES
RKEY         : 50000000000
NBIT         : 256
MAX FOUND    : 65536
HASH160 FILE : 01.bin
OUTPUT FILE  : Found.txt

Loading      : 100 %
Loaded       : 1,326,779 address

Bloom at 0000027BC6A6F2E0
  Version    : 2.1
  Entries    : 2653558
  Error      : 0.0000010000
  Bits       : 76303525
  Bits/Elem  : 28.755175
  Bytes      : 9537941 (9 MB)
  Hash funcs : 20

Start Time   : Sat Aug  7 01:36:04 2021
Base Key     : Reloads random hashes every 50000000000

GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(288x256)
  Project Rotor-CUDA         :   https://github.com/phrutis/Rotor-Cuda
  Donate BitCoin             :   bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

 [4056532AFE500B74E8B197DC6993633490ED268508A6AE0E91FD0A27E23634D1] [00:02:12] [F: 0] [T: 101,015,617,536] [CPU+GPU: 0.00 Mk/s]
 ```
## Building
- Microsoft Visual Studio Community 2019
- CUDA version 10.22
## Donation
- BTC: bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

