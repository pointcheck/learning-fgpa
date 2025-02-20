# Тележка управляемая по ИК (IR) от стандартного пульта ДУ

## Цель проекта

Освоение процесса разработки цифровых схем и конечных автоматов на языке Verilog.

## Задача

Разработать конечный автомат на HDL языке Verilog для удаленного управления подвижной тележкой в составе которой имеется:
- один электромотор с редуктором и приводом на задние колёса;
- один H-мост для управления электромотором.
- один серво-привод [Hitec HS-325HB](https://servodatabase.com/servo/hitec/hs-325hb) для поворота передних колес;
- приемник ИК сигнала [TSOP4336](https://static.chipdip.ru/lib/016/DOC029016184.pdf) (36kHz);
- пара ИК светодиод + ИК фотодиода для определения наличия препятствия;
- плата [TangNano-9K](https://wiki.sipeed.com/hardware/en/tang/Tang-Nano-9K/Nano-9K.html) с микросхемой ПЛИС Gowin GW1NR-9K;
- батарея номинальным напряжением 6В (4xAAA).

## Алгоритм

КА в составе ПЛИС принимате входной сигнал от ИК приемника закодированный в формате RC5, SIRC или NEC, декодирует его и в зависимости от полученной команды (нажатой клавиши) выполняет следующие действия:
- если нажата клавиша "вверх", то включает мотор на движение вперед;
- если нажата клавиша "вних", то влючает мотор на движение назад;
- если нажата клавиша "влево", то увеличиывает (или уменьшает) скважность ШИМ сигнала на серво-привода для поворота колес влево;
- если нажата клавиша "вправо", то уменьшает (или увеличивает) скважность ШИМ сигнала на серво-привода для поворота колес вправо;
- если нажата клавиша "RETURN", то производится останов вращения двигателя;
- если с ИК фото-диода поступает сигнал, то КА останавливает и недопускает движения вперед, при этом движение назад возможно.

### Управление RC серво-приводом

Станадартные RC-серво управляются шириной импульса, который формируется на её входе с определенной периодичность. Это похоже на ШИМ с апериодичным сигналом. При длительности импульса в 1,5 мс серво устанавливает ротор в нулевое (центральное) положение. При подаче импульса длительностью меньше чем 1,5мс, серво-привод уменьшает угол положения ротора (вращает ротор против часовой стрелки). При подаче импульса длительностью больше чем 1,5мс, серво-привод увеличивает угол положения ротора.

![Алгоритм работы RC-серво](https://upload.wikimedia.org/wikipedia/commons/thumb/6/6c/Servomotor_Timing_Diagram.svg/330px-Servomotor_Timing_Diagram.svg.png)

Статья на Wikipedia: https://en.wikipedia.org/wiki/Servo_control

### Прием и декодирование ИК (IR) сигнала

Существуюет несколько сопособов кодирования сигнала для пультов ДУ.

1. Sony SIRC Protocol

В аппаратуре производства Sony используется SIRC протокол, в котором логические "0" и "1" закодированны разной длительностью импульса, что предстваляет собой PDM-кодирование.

![Sony SIRC Protocol](https://labprojectsbd.com/wp-content/uploads/2021/03/c2-768x300.png)

Описание протокола: https://faculty-web.msoe.edu/johnsontimoj/Common/FILES/sony_sirc_protocol.pdf


2. NEC IR Protocol

Используется PDM-кодирование. Длина посылки - 32 бита.

![NEC IR Protocol](https://exploreembedded.com/wiki/images/a/ac/NecIrRemote_0.png)

Описание протокола: https://exploreembedded.com/wiki/NEC_IR_Remote_Control_Interface_with_8051


## Ссылки

1. Статья на Хабре "[IR remote control, а без микроконтроллеров можно? Да не вопрос](https://habr.com/ru/companies/timeweb/articles/784500/)"
2. Статья на Wikipedia "[RC-5 Protocol](https://en.wikipedia.org/wiki/RC-5)"
3. Cтатья "[Система ИК ДУ с кодированием](https://radiostorage.net/512-sistema-ik-du-s-kodirovaniem.html)"
   
