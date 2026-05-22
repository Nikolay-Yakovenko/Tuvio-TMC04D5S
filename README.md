# Tuvio-TMC04D5S
Подключение мультиварки Tuvio-TMC04D5S к Home Assistant посредством Tuya Local.

Для подкючения необходимо файл ```multicooker_tuvio_tmc04d5s.yaml``` поместить в папку ```config\custom_components\tuya_local\devices```  и перезагрузить интеграцию.

Соответственно мультиварка должна быть подключена к сети WiFi и получен локальный ключ 
(как его получить можно почитать в описании к интеграции Tuya Local).

Т.к. при обновлении интеграции сначала удаляются все файлы yaml, то можно добавить команду на восстановление файла ```multicooker_tuvio_tmc04d5s.yaml```

Для этого надо в файле ```configuration.yaml``` сделать раздел ```shell_command```, и прописать там команду на копирование. Сам файл ```multicooker_tuvio_tmc04d5s.yaml``` надо положить в папку ```config\custom_components\```

```yaml
shell_command:
  copy_file_to_tuya_local_devices: cp /config/custom_components/multicooker_tuvio_tmc04d5s.yaml /config/custom_components/tuya_local/devices/multicooker_tuvio_tmc04d5s.yaml
```

После чего создать автоматизацию, которая при выключении Home Assistant будет запускать эту shell-команду.

Пример автоматизации:
```yaml
alias: Системное. Копирование файла устройства в Tuya_Local после обновления 2.
description: >-
  Возвращаем поддержку мультиварки в интеграцию Tuya_Local после ее обновления.
  При обновлении все файлы интеграции (в том числе и мой, с поддержкой
  мультиварки) удаляются. Поэтому после обновления копируем файл снова.
triggers:
  - event: shutdown
    trigger: homeassistant
conditions: []
actions:
  - action: shell_command.copy_file_to_tuya_local_devices
    metadata: {}
    data: {}
mode: single
```