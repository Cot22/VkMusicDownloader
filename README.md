# VK Music Downloader
Проект предназначен для использования в социальной сети [ВКонтакте](https://vk.com).

Позволяет:
1. Скачивать аудиозаписи средствами браузера из списка аудиозаписей пользователя, либо из плейлиста.
2. Сохранить список аудиозаписей в файл и загрузить этот файл на компьютер. В файле будет список из исполнителя и названия песни и URL, по которому находится mp3 файл песни. Песня и её URL разделены знаком [табуляции](https://ru.wikipedia.org/wiki/Табуляция) – `\t`. (Однако вк динамически создаёт URL при воспроизведении аудио, поэтому найти файл по этому адресу можно будет только в течении ограниченного времени. Потом Вы получите [404](https://ru.wikipedia.org/wiki/HTTP_404)).
3. Загрузить аудиозаписи, сохранённые в файл средствами скрипта на языке `python3`. Для этого на компьютере должен быть установлен интерпретатор python.

## Инструкция
### 1. Установка
Если Вы хотите просто скачать аудиозаписи средствами браузера ([2.1](#21-Загрузка-аудиозаписей-средствами-браузера)) или сохранить список аудиозаписей как текстовый файл ([2.2](https://github.com/artslob/VkMusicDownloader#22-Сохранение-списка-аудиозаписей-в-текстовый-файл)), можете пропустить этот пункт.
1. Скачайте исходный код проекта. Это можно сделать 2 способами:  
   * Скачайте и разархивируйте [zip-архив](../../archive/master.zip).
   * Либо используйте git:
   ```
   git clone https://github.com/artslob/VkMusicDownloader.git
   ```
2. Если Вы будете использовать питоновский скрипт:

   Убедитесь, что у Вас есть интерпретатор python3 и он доступен из командной строки:
   ```bash
   # linux
   which python3
   
   # windows
   where python3
   ```
   Затем установим виртуальное окружение:
   ```bash
   # unix
   cd VkMusicDownloader
   python3 -m venv vk_venv
   source vk_venv/bin/activate
   python3 -m pip install -r requirements.txt
   
   # windows
   cd VkMusicDownloader
   python3 -m venv vk_venv
   vk_venv\Scripts\activate.bat
   python3 -m pip install -r requirements.txt
   ```
### 2. Использование
#### 2.1 Загрузка аудиозаписей средствами браузера
1. Открываем содержимое файла [vk-audio-downloader.js](../../raw/master/vk-audio-downloader.js) (:point_left: перейдите по этой ссылке). Выделяем весь текст (`Ctrl+A`) и копируем.
1. Заходим в раздел с аудиозаписями.
1. Листаем в самый низ. (Чтобы прогрузились все аудиозаписи) (Можно зажать клавишу PageDown)
1. Открываем консоль браузера. (`F12` -> Консоль или `Ctrl+Shift+J`)
1. Вставляем код.
1. Пролистайте в самый верх вставленного Вами кода. Найдите строку: `var VK_DOWNLOADER_HANDLER_TYPE = 'TEXT';`. Замените `TEXT` на `FILE`. Должно получиться `var VK_DOWNLOADER_HANDLER_TYPE = 'FILE';`.
1. Если хотите загрузить только N первых (недавно добавленных) аудиозаписей, то найдите строку `var VK_DOWNLOADER_DOWNLOAD_LATEST = 0;`. Замените `0` на нужное число. Например: `var VK_DOWNLOADER_DOWNLOAD_LATEST = 50;`, тогда загрузятся только 50 первых в списке аудиозаписей.
1. Нажмите Enter. Скачивание началось...
1. Браузер может потребовать разрешение на сохранение файлов, необходимо подтвердить действие.
1. Оставляем браузер на время прямо пропорциональное количеству аудиозаписей. В консоли будет отображаться прогресс скачивания.

#### 2.2 Сохранение списка аудиозаписей в текстовый файл.
Точно так же, как и [загрузка аудиозаписей средствами браузера](#21-Загрузка-аудиозаписей-средствами-браузера), только не нужно изменять `TEXT` на `FILE` в строке `var VK_DOWNLOADER_HANDLER_TYPE = 'TEXT';`.  
В конце загрузки должно появиться окно с предложением скачать текстовый файл `audios.txt`. Если окно не появилось, то листните вниз страницы, там, скорее всего внизу и слева, будет ссылка с названием `audios.txt`. Нажмите на неё.

#### 2.3 Загрузка из тестового файла с помощью python скрипта.
После того, как Вы скачали [файл](#22-Сохранение-списка-аудиозаписей-в-текстовый-файл) со списком аудиозаписей и их URL, а также [установили окружение](#1-Установка), Вы можете использовать скрипт.  
Чтобы получить помощь по параметрам скрипта, выполните команду: `python3 main.py -h`.
Скрипт будет использовать параметры по умолчанию, если не указывать флаги.
Пример запуска:
```bash
python3 main.py -f /some/directory/audios.txt -d /where/to/save/audios/ -vv
```
`-f` – путь к файлу со списком аудиозаписей. По умолчанию ищется файл с названием `audios.txt` в той же директории, где лежит скрипт `main.py`.  
`-d` – путь к директории, в которую будут загружены аудиозаписи. По умолчанию это та же директория, где лежит скрипт `main.py`.  
`-vv` – означает высокий уровень логирования. Скрипт будет выводить полную информацию о выполнении на [стандартный вывод](https://ru.wikipedia.org/wiki/Стандартные_потоки#Стандартный_вывод). Можно сохранять лог в файл с помощью параметра `-l`.  

Текущий help скрипта (когда скрипт находится в папке `/tmp`):
```
usage: main.py [-h] [-d DIR] [-f FILE] [-n NUMBER] [-l LOGFILE] [-v]

Download audios from text file with links.

optional arguments:
  -h, --help            show this help message and exit
  -d DIR, --dir DIR     Destination directory where audios are saved. Default:
                        /tmp.
  -f FILE, --file FILE  File that contains names of songs and their urls. Can
                        be set to stdin with value '-'. Default:
                        /tmp/audios.txt.
  -n NUMBER, --number NUMBER
                        Number of files that downloading at the same time.
                        Default: 10.
  -l LOGFILE, --logfile LOGFILE
                        Writable file for logging. By default set to stdout
                        with value '-'.
  -v, --verbose         Set level of logging. You can increase level by
                        repeating key. Levels by number: default: WARNING, -v:
                        INFO, -vv: DEBUG
```
## Лицензия
[MIT License](LICENSE.txt)
