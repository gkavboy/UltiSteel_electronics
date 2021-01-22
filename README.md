# UltiSteel_electronics
Electronics update for Ulti Steel 3D printer

------

:warning: Автор не несет ответственности за неправильную сборку, выбор комплектующих и любую порчу имущества и здоровья, вызванную эксплуатацией описываемых плат.

------

## UlTi_driver_ext_board

Плата представляет собой модуль расширения, устанавливаемый на свободное место, предназначенное для установки драйвера мотора.

### Возможности платы:

- 1 канал управления, включеный по умолчанию;
- 2 канала управления, включенных по умолчанию;
- Максимальная нагрузка на канал - 1А;
- Максимальное напряжение питания  двигателей принтера  - 24В.

### Принцип работы:

Плата использует три линии связи с процессором материнской платы, которые соответствуют линиям **EN**, **STEP**, **DIR** платы драйвера мотора. Линии **EN** соответствуе выход **!FAN**, **STEP** - **FAN1**, **DIR** - **FAN2**. 

Ко всем трем линиям подключены маломощные MOSFET транзисторы типа **IRLML6344** или аналогичных с минимальным пороговым напряжением затвора и приемлемыми характеристиками канала при напряжении на затворе 3.3 - 5.0В.

Несмотря на то, что по документации подобные транзисторы рассчитаны на ток 4-5А через канал, я не рекомендую превышать значение 1А, поскольку у применяемых транзисторов минимальное охлаждение корпуса. Таким образом, подключаемая к каждому каналу управления может потреблять не более 12Вт при напряжении питания моторов принтера 12В и 24Вт при напряжении питания моторов принтера 24В. В эти значения укладываются как светодиодные ленты длиной 1М, так и большинство кулеров охлаждения.

Канал 1 (**!FAN**) при включении питания принтера будет всегда включен. Это удобно для подключения подсветки рабочей зоны или охлаждения электроники принтера. Каналы 2 и 3 (**FAN1**, **FAN2**) при подаче питания выключены. После запуска управляющей программы принтера состояние каналов может быть изменено.

Каналы **FAN1** и **FAN2** можно использовать в режиме ШИМ для регулирования выдаваемой в нагрузку мощности, канал **!FAN** рекомендуется использовать только в режиме вкл/выкл.

### Конфигурация ПО принтера:

Поскольку на принтерах может быть использовано разное ПО и разные материнские платы, я не могу указывать все варианты конфигурирования. Здесь будут рассмотрены только варианты подключения к платам Bigtreetech SKR 1.3 и MKS GenL V2 с прошивкой Klipper. 

#### Bigtreetech SKR 1.3

```
#-----------PINOUT-----------

#EN	P0.10	!FAN

#STEP	P0.1	FAN1

#DIR	P0.0	FAN2

#----------------------------

#!FAN config as output pin

#

#[output_pin light]

#pin: P0.10

#pwm: true

#value: 0.5

#shutdown_value: 1.0

#cycle_time: 0.01

#----------------------------

#FAN1 config as output pin

#

#[output_pin firstfan]

#pin: P0.1

#pwm: true

#value: 0.5

#shutdown_value: 1.0

#cycle_time: 0.01

#----------------------------

#FAN1 config as generic fan

#

#[fan_generic firstfan]

#pin: P0.1

#max_power: 1.0

#shutdown speed: 1.0

#cycle_time: 0.01

#----------------------------
```


#### MKS GenL V2

printer.cfg

------

## UlTi_head_connector

### Возможности платы

Для разработки платы за основу была взята плата от разработчиков принтера Ulti Steel. По сравнению с оригинальным вариантом сокращено количество проводов для подключения и уменьшен ассортимент используемых разъемов для подключения периферии.

Реализованы следующие возможности:

- Подключение нагревательного элемента через разъем XT-30;
- Подключение термистора через разъем JST XH 2.54;
- Подключение кулера обдува радиатора хотэнда через разъем JST XH 2.54;
- Подключение двух клеров обдува модели через разъемы JST XH 2.54.
- Подключение к материнской плате принтера всего 6 проводами.

### Принцип работы

Плата представляет собой простой переходник с проводов на разъемы. Уменьшение числа проводов для подключения платы к материнской плате принтера достигнуто за счет того, что + питания кулеров и нагревателя хотэнда - это одна и та же линия питания. А потребление кулеров по сравнению с нагревателем хотэнда незначительно. 

В связи с этим было принято решение провести к плате принтера 2 провода (+ и - нагревателя хотэнда) сечением 0.75мм^2 (родной провод нагревателя хотэнда), а для подключения кулеров использовать 2 провода сечением 0.2мм^2, подключаемых к материнской плате в разъемы, указанные в руководстве по сборке принтера Ulti Steel. Термистор подключается в свое гнездо на материнской плате так же двумя проводами сечением 0.2мм^2. 

### Подключение

Для корректного подключения с плате Bigtreetech SKR 1.3 следует использовать схему, приведенную в файле **Connection to skr.pdf**, который находится в папке **pdf**.

Подключение проводится только при полностью обесточенном принтере.

:warning: ​После подключения платы перед подачей питания настоятельно рекомендуется перепроверить правильность подключения.