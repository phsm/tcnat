# Управление RAW NAT с помощью tcnat
Python-скрипт tcnat облегчает создание и удаление правил raw nat, формируя
и выполняя команды tc. TC RAW NAT используется для быстрой трансляции адресов,
не прибегая к conntrack.

[Скачать](http://gitlab.nek0.net/phsm/tcnat/raw/master/tcnat)

## Настройка
В коде скрипта есть несолько конфигурационных констант:
* `NAT_IFACE` - сетевой интерфейс, на котором будет осуществляться и source, и destination NAT
* `TC_PATH` - путь к утилите tc (если вдруг она не находится по стандартному пути)
* `WHITELISTS_SRC` - список ipset-ов с белыми src ip/src net. Трафик для src ip, входящих в данные ipset-ы НЕ будет транслироваться


## Команды
### tcnat list_srcnat
Выводит список установленных правил source NAT на интерфейсе `NAT_IFACE`.
Поле `Pref` - приоритет правила (можно сказать его ID). По нему правила удаляются.
Поле `NAT FROM` - Source IP, который будет транслирован
Поле `NAT TO` - Новый source IP, который будет после трансляции

### tcnat list_dstnat
Выводит список установленных правил destination NAT на интерфейсе `NAT_IFACE`.
Поле `Pref` - приоритет правила (можно сказать его ID). По нему правила удаляются.
Поле `NAT FROM` - Destination IP, который будет транслирован
Поле `NAT TO` - Новый destination IP, который будет после трансляции

### tcnat add_srcnat &lt;src_ip&gt; &lt;translated_ip&gt;
Добавляет новое правило source NAT: адрес отправки пакета будет заменён с src_ip на translated_ip

### tcnat add_dstnat &lt;dst_ip&gt; &lt;translated_ip&gt;
Добавляет новое правило destination NAT: адрес назначения пакета будет заменён с dst_ip на translated_ip

### tcnat del_srcnat &lt;pref&gt;
Удаляет правило source NAT с pref &lt;pref&gt;, это значение можно посмотреть командой `tcnat list_srcnat`

### tcnat del_dstnat &lt;pref&gt;
Удаляет правило destination NAT с pref &lt;pref&gt;, это значение можно посмотреть командой `tcnat list_dstnat`
