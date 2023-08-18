

# Generative Agents: Interactive Simulacra of Human Behavior 

<p align="center" width="100%">
<img src="cover.png" alt="Smallville" style="width: 80%; min-width: 300px; display: block; margin: auto;">
</p>

Данный репозиторий сопровождает нашу научную работу "Генеративные агенты: интерактивные симулякры человеческого поведения" (https://arxiv.org/abs/2304.03442). Он содержит наш основной модуль моделирования генеративных агентов - вычислительных агентов, имитирующих правдоподобное поведение человека, и их игровую среду. Ниже описаны шаги по настройке среды симуляции на локальной машине и воспроизведению симуляции в виде демонстрационной анимации.

## <img src="https://joonsungpark.s3.amazonaws.com:443/static/assets/characters/profile/Isabella_Rodriguez.png" alt="Generative Isabella">   Настройка среды 
Для настройки среды необходимо сгенерировать файл `utils.py`, содержащий ваш ключ OpenAI API, и загрузить необходимые пакеты.

### Step 1. Generate Utils File
В папке `reverie/backend_server` (где находится `reverie.py`) создайте новый файл с именем `utils.py` и скопируйте и вставьте в него содержимое, приведенное ниже:
```
# Скопируйте и вставьте свой ключ API OpenAI
openai_api_key = "<Your OpenAI API>"
# Поставьте свое имя
key_owner = "<Name>"

maze_assets_loc = "../../environment/frontend_server/static_dirs/assets"
env_matrix = f"{maze_assets_loc}/the_ville/matrix"
env_visuals = f"{maze_assets_loc}/the_ville/visuals"

fs_storage = "../../environment/frontend_server/storage"
fs_temp_storage = "../../environment/frontend_server/temp_storage"

collision_block_id = "32125"

# Verbose 
debug = True
```
Замените `<Ваш OpenAI API>` на Ваш ключ OpenAI API, а `<имя>` на Ваше имя.

** Bug-fix: Добавьте папку movement в environment/frontend_server/storage
 
### Шаг 2. Установите файл `requirements.txt
Установите все, что перечислено в файле `requirements.txt` (настоятельно рекомендую предварительно настроить virtualenv, как обычно). Примечание по версии Python: мы тестировали нашу среду на Python 3.9.12. 

## <img src="https://joonsungpark.s3.amazonaws.com:443/static/assets/characters/profile/Klaus_Mueller.png" alt="Generative Klaus">   Запуск моделирования 
Чтобы запустить новую симуляцию, необходимо одновременно запустить два сервера: сервер среды и сервер симуляции агента.

### Шаг 1. Запуск сервера окружения
Опять же, среда реализована как Django-проект, и поэтому необходимо запустить Django-сервер. Для этого сначала перейдите в командной строке по адресу `environment/frontend_server` (именно там находится файл `manage.py`). Затем выполните следующую команду:

    python manage.py runserver

Затем в своем любимом браузере перейдите по адресу [http://localhost:8000/](http://localhost:8000/). Если вы увидите сообщение "Your environment server is up and running", значит, ваш сервер работает правильно. Убедитесь, что сервер окружения продолжает работать во время выполнения моделирования, поэтому держите эту вкладку командной строки открытой! (Примечание: я рекомендую использовать Chrome или Safari. Firefox может давать некоторые сбои во внешнем интерфейсе, но это не должно мешать реальному моделированию).

### Шаг 2. Запуск сервера моделирования
Откройте другую командную строку (та, которую вы использовали в шаге 1, должна по-прежнему запускать сервер окружения, поэтому оставьте ее как есть). Перейдите по адресу `reverie/backend_server` и запустите файл `reverie.py`.

    python reverie.py
Это приведет к запуску сервера моделирования. Появится приглашение командной строки с запросом: "Введите имя развиваемой симуляции: ". Чтобы запустить трехагентную симуляцию с Изабеллой Родригес, Марией Лопес и Клаусом Мюллером, введите следующее:
    
    base_the_ville_isabella_maria_klaus
Затем появится запрос: "Введите имя новой симуляции: ". Введите любое имя, обозначающее текущую симуляцию (например, пока подойдет просто "test-simulation").

    test-simulation
**Bug-fix: На этом этапе он выдаст следующее приглашение: "Введите опцию: ". Перед тем как это сделать, найдите папку с названием вашей симуляции в environment/frontend_server/storage и добавьте внутрь ее папку movement

### Шаг 3. Запуск и сохранение моделирования
В браузере перейдите по адресу [http://localhost:8000/simulator_home](http://localhost:8000/simulator_home). Вы должны увидеть карту Смолвиля, а также список активных агентов на карте. Перемещаться по карте можно с помощью стрелок клавиатуры. Пожалуйста, держите эту вкладку открытой. Для запуска симуляции введите на сервере симуляции следующую команду в ответ на запрос "Введите опцию":

    run <step-count>
Обратите внимание на то, что вместо `<step-count>` необходимо ввести целое число, обозначающее количество игровых шагов, которые необходимо смоделировать. Например, если требуется смоделировать 100 игровых шагов, то следует ввести `run 100`. Один игровой шаг равен 10 секундам в игре.


Симуляция должна быть запущена, и вы увидите, как агенты перемещаются по карте в браузере. После завершения работы симуляции снова появится запрос "Ввод опции". В этот момент можно выполнить дополнительные шаги, повторно введя команду run с желаемыми шагами игры, выйти из симуляции без сохранения, набрав `exit`, или сохранить и выйти, набрав `fin`.

К сохраненной симуляции можно получить доступ при следующем запуске сервера симуляций, указав имя симуляции в качестве имени развилки. Это позволит запустить симуляцию с того места, на котором вы остановились.

### Шаг 4. Воспроизведение моделирования
Для воспроизведения уже запущенной симуляции достаточно иметь запущенный сервер окружения и перейти в браузере по следующему адресу: `http://localhost:8000/replay/<simulation-name>/<starting-time-step>`. Пожалуйста, замените `<simulation-name>` на имя симуляции, которую вы хотите воспроизвести, а `<starting-time-step>` на целочисленный временной шаг, с которого вы хотите начать воспроизведение.

Например, перейдя по следующей ссылке, вы запустите предварительно смоделированный пример, начиная с временного шага 1:  
[http://localhost:8000/replay/July1_the_ville_isabella_maria_klaus-step-3-20/1/](http://localhost:8000/replay/July1_the_ville_isabella_maria_klaus-step-3-20/1/)

### Шаг 5. Демонстрация симуляции
Вы могли заметить, что все спрайты персонажей в реплее выглядят одинаково. Хотелось бы пояснить, что функция повтора предназначена в первую очередь для отладки и не ставит своей приоритетной задачей оптимизацию размера папки с симуляцией или визуального оформления. Чтобы правильно продемонстрировать симуляцию с соответствующими спрайтами персонажей, необходимо сначала сжать симуляцию. Для этого откройте текстовым редактором файл `compress_sim_storage.py`, расположенный в каталоге `reverie`. Затем выполните функцию `compress`, указав в качестве входных данных имя целевой симуляции. В результате файл симуляции будет сжат и готов к демонстрации.

Для запуска демонстрации перейдите в браузере по следующему адресу: `http://localhost:8000/demo/<simulation-name>/<starting-time-step>/<simulation-speed>`. Обратите внимание, что `<simulation-name>` и `<starting-time-step>` обозначают одно и то же, как было сказано выше. `<simulation-speed>` может быть задана для управления скоростью демонстрации, где 1 - самая медленная, а 5 - самая быстрая. Например, при переходе по следующей ссылке будет запущен предварительно смоделированный пример, начинающийся с шага 1, со средней скоростью демонстрации:  
[http://localhost:8000/demo/July1_the_ville_isabella_maria_klaus-step-3-20/1/3/](http://localhost:8000/demo/July1_the_ville_isabella_maria_klaus-step-3-20/1/3/)

### Советы
Мы заметили, что API OpenAI может зависать при достижении лимита почасовой скорости. В этом случае может потребоваться перезапуск симуляции. На данный момент мы рекомендуем часто сохранять симуляцию по мере ее выполнения, чтобы в случае необходимости остановить и запустить ее заново, как можно меньше потерять. Запуск таких симуляций, по крайней мере, в начале 2023 года, может быть несколько дорогостоящим, особенно при наличии большого количества агентов в среде.

## <img src="https://joonsungpark.s3.amazonaws.com:443/static/assets/characters/profile/Maria_Lopez.png" alt="Generative Maria">   Расположение хранилища симуляций
Все сохраняемые симуляции будут находиться в каталоге `environment/frontend_server/storage`, а все сжатые демо-версии - в каталоге `environment/frontend_server/compressed_storage`. 

## <img src="https://joonsungpark.s3.amazonaws.com:443/static/assets/characters/profile/Sam_Moore.png" alt="Generative Sam">   Customization

Существует два способа дополнительной настройки имитационных моделей. 

### Автор и загрузка истории агентов
Во-первых, в начале моделирования необходимо инициализировать агентов с уникальной историей. Для этого необходимо: 1) начать симуляцию, используя одну из базовых симуляций, и 2) создать и загрузить историю агентов. Более конкретно, вот шаги:

#### Шаг 1. Запуск базовой симуляции 
В репозиторий включены две базовые симуляции: `base_the_ville_n25` с 25 агентами и `base_the_ville_isabella_maria_klaus` с 3 агентами. Загрузите одну из базовых симуляций, выполнив действия до шага 2 выше. 

#### Шаг 2. Загрузка файла истории 
Затем, получив запрос "Enter option: ", необходимо загрузить историю агента, выполнив следующую команду:

    call -- load history the_ville/<history_file_name>.csv
Обратите внимание, что необходимо заменить `<имя_файла_истории>` на имя существующего файла истории. В качестве примера в репозиторий включены два файла истории: `agent_history_init_n25.csv` для `base_the_ville_n25` и `agent_history_init_n3.csv` для `base_the_ville_isabella_maria_klaus`. Эти файлы содержат разделенные точкой с запятой списки записей памяти для каждого из агентов - их загрузка приведет к вставке записей памяти в поток памяти агентов.

#### Step 3. Дальнейшая настройка 
Чтобы настроить инициализацию, создав свой собственный файл истории, поместите его в следующую папку: `environment/frontend_server/static_dirs/assets/the_ville`. Формат столбцов вашего собственного файла истории должен соответствовать приведенным примерам файлов истории. Поэтому мы рекомендуем начать процесс с копирования и вставки тех, которые уже есть в репозитории.

### Создание новых базовых симуляций
Для более сложной настройки необходимо создать собственные файлы базовых симуляций. Наиболее простой подход заключается в копировании и вставке существующей папки с базовыми симуляциями, переименовании и редактировании ее в соответствии с вашими требованиями. Этот процесс будет проще, если вы решите оставить имена агентов без изменений. Однако если вы захотите изменить их имена или увеличить количество агентов на карте Smallville, то, возможно, вам придется напрямую редактировать карту с помощью редактора карт [Tiled](https://www.mapeditor.org/).


## <img src="https://joonsungpark.s3.amazonaws.com:443/static/assets/characters/profile/Eddy_Lin.png" alt="Generative Eddy">   Authors and Citation 

**Authors:** Joon Sung Park, Joseph C. O'Brien, Carrie J. Cai, Meredith Ringel Morris, Percy Liang, Michael S. Bernstein

Please cite our paper if you use the code or data in this repository. 
```
@inproceedings{Park2023GenerativeAgents,  
author = {Park, Joon Sung and O'Brien, Joseph C. and Cai, Carrie J. and Morris, Meredith Ringel and Liang, Percy and Bernstein, Michael S.},  
title = {Generative Agents: Interactive Simulacra of Human Behavior},  
year = {2023},  
publisher = {Association for Computing Machinery},  
address = {New York, NY, USA},  
booktitle = {In the 36th Annual ACM Symposium on User Interface Software and Technology (UIST '23)},  
keywords = {Human-AI interaction, agents, generative AI, large language models},  
location = {San Francisco, CA, USA},  
series = {UIST '23}
}
```

## <img src="https://joonsungpark.s3.amazonaws.com:443/static/assets/characters/profile/Wolfgang_Schulz.png" alt="Generative Wolfgang">   Acknowledgements

We encourage you to support the following three amazing artists who have designed the game assets for this project, especially if you are planning to use the assets included here for your own project: 
* Background art: [PixyMoon (@_PixyMoon\_)](https://twitter.com/_PixyMoon_)
* Furniture/interior design: [LimeZu (@lime_px)](https://twitter.com/lime_px)
* Character design: [ぴぽ (@pipohi)](https://twitter.com/pipohi)

In addition, we thank Lindsay Popowski, Philip Guo, Michael Terry, and the Center for Advanced Study in the Behavioral Sciences (CASBS) community for their insights, discussions, and support. Lastly, all locations featured in Smallville are inspired by real-world locations that Joon has frequented as an undergraduate and graduate student---he thanks everyone there for feeding and supporting him all these years.


