# VK Music Downloader
Проект предназначен для использования в социальной сети ВКонтакте.  
Перед использованием обязательно прочтение дисклеймера ([тык](#Дисклеймер)).

Позволяет:
1. Скачивать аудиозаписи средствами браузера из списка аудиозаписей пользователя, либо из плейлиста.
2. Сохранить список аудиозаписей в файл и загрузить этот файл на компьютер. В файле будет список из исполнителя и названия песни и URL, по которому находится mp3 файл песни. Песня и её URL разделены знаком [табуляции](https://ru.wikipedia.org/wiki/Табуляция) – `\t`. (Однако вк динамически создаёт URL при воспроизведении аудио, поэтому найти файл по этому адресу можно будет только в течении ограниченного времени. Потом Вы получите [404](https://ru.wikipedia.org/wiki/HTTP_404)).
3. Загрузить аудиозаписи, сохранённые в файл средствами скрипта на языке `python3`. Для этого на компьютере должен быть установлен интерпретатор python.

## Инструкция
### 1. Установка
Если Вы хотите просто скачать аудиозаписи средствами браузера ([2.1](#21-Загрузка-аудиозаписей-средствами-браузера)) или сохранить список аудиозаписей как текстовый файл ([2.2](#22-Сохранение-списка-аудиозаписей-в-текстовый-файл)), можете пропустить этот пункт.
1. Скачайте исходный код проекта. Это можно сделать 2 способами:  
   * Скачайте и разархивируйте [zip-архив](../../archive/master.zip).
   * Либо используйте git:
   ```bash
   git clone https://github.com/artslob/VkMusicDownloader.git
   ```
2. Убедитесь, что у Вас есть интерпретатор python3.6 (возможно, подойдёт и меньшая версия `3.x`) и он доступен из командной строки:
   ```bash
   # linux
   which python3
   python3 --version
   
   # windows
   where python3
   python3 --version
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
1. Открываем содержимое файла vk-audio-downloader.js: [ссылка](../../raw/master/js-handlers/vk-audio-downloader-file.js). Выделяем весь текст (`Ctrl+A`) и копируем.
1. Заходим в раздел с аудиозаписями.
1. Листаем в самый низ. (Чтобы прогрузились все аудиозаписи) (Можно зажать клавишу PageDown)
1. Открываем консоль браузера. (`F12` -> Консоль или `Ctrl+Shift+J`)
1. Вставляем код.
1. Если хотите загрузить только N первых (недавно добавленных) аудиозаписей, то пролистайте в самый верх вставленного Вами кода и найдите строку `var VK_DOWNLOADER_DOWNLOAD_LATEST = 0;`. Замените `0` на нужное число. Например: `var VK_DOWNLOADER_DOWNLOAD_LATEST = 50;`, тогда загрузятся только 50 первых в списке аудиозаписей.
1. Нажмите Enter. Скачивание началось...
1. Браузер может потребовать разрешение на сохранение файлов, необходимо подтвердить действие.
1. Оставляем браузер на время прямо пропорциональное количеству аудиозаписей. В консоли будет отображаться прогресс скачивания.

#### 2.2 Сохранение списка аудиозаписей в текстовый файл.
Точно так же, как и в загрузке аудиозаписей [средствами браузера](#21-Загрузка-аудиозаписей-средствами-браузера), только нужно скопировать содержимое другого файла: [ссылка](../../raw/master/js-handlers/vk-audio-downloader-text.js).  
В конце загрузки должно появиться окно с предложением скачать текстовый файл `audios.txt`. Если окно не появилось, то листните вниз страницы, там, скорее всего внизу и слева, будет ссылка с названием `audios.txt`. Нажмите на неё.

#### 2.3 Загрузка из тестового файла с помощью python скрипта.
После того, как Вы скачали [файл](#22-Сохранение-списка-аудиозаписей-в-текстовый-файл) со списком аудиозаписей и их URL, а также установили окружение и активировали его ([тык](#1-Установка)), Вы можете использовать скрипт.  
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

## Благодарности
[@abler](https://github.com/abler98) за оригинальный [скрипт](https://gist.github.com/abler98/2fbeee9f7bd056bbe6c485041370c556).  
[@fizvlad](https://github.com/fizvlad) за [форк](https://gist.github.com/fizvlad/4c2eb98b5fb12a6a975d27b0bfcd9fcf) оригинального скрипта, с которого был сделан форк [vk-audio-downloader.js](vk-audio-downloader.js).  

## Дисклеймер
1. Социальная сеть ВКонтакте, о которой говорится в этом README и остальных файлах проекта, является вымышленной социальной сетью и не имеет никакого отношения к [vk.com](https://vk.com). Все действия и инструкции, представленные на этой странице, не имеют никакого отношения к реальной жизни.
1. Исходный код является открытым; информация представленная в нем предназначена для ознакомления пользователей с материалами, которые могут представлять для них интерес.
1. Доступ к ресурсу и исходному коду, а также использование материалов осуществляются исключительно по Вашему усмотрению и без предоставления каких-либо гарантий.
1. Автор(ы) кода не несут никакой ответственности за использование, распространение и *прочую* деятельность *прочих* лиц, связанную с представленными выше материалами.
1. Чтение, исправление, размножение и другие действия с контентом, который размещается на ресурсе, может нарушать законодательство страны Вашего проживания, а также пользовательское соглашение, заключенное между Вами и вымешленной и, возможно, реальной социальной сетью ВКонтакте.
1. Весь текст данных файлов создан с помощью случайной генерации текста. ~~Честно~~

## Ave, Caesar, morituri te salutant
![durov](https://user-images.githubusercontent.com/16637122/46695946-9a504280-cc19-11e8-95a3-02d1d97fe01a.jpg "Make vk great again!")
