# GyverLibs
## Написанные с нуля/модифицированные (AlexGyver) библиотеки для Arduino
### На данный момент содержит:
- **GyverHacks (текущая 1.3)** - библиотека с некоторыми удобными хаками:
	+ **GTimer** - компактная альтернатива конструкции таймера с millis()
		+ Класс GTimer (period) - установка периода
		+ setInterval(period) - настройка периода вызова
		+ reset() - сброс
	+ **GFilterRA** - компактная альтернатива фильтра бегущее среднее (Running Average)
		+ Класс GFilterRA
		+ setCoef(coef) - настройка коэффициента фильтрации (0.00-1.00)
		+ setStep (step) - настройка шага дискретизации (период фильтрации) - встроенный таймер! миллисекунды
		+ filtered(value) - возвращает фильтрованное значение
		+ filteredTime(value) - возвращает фильтрованное с опорой на встроенный таймер
	+ **GParsingStream** - парсинг данных из Serial
		+ parsingStream((int*)&intData); - автоматическая расфасовка пакетов вида **$110 25 600 920;** в массив intData
		+ dataReady() - функция-флаг принятия нового пакета данных
		+ Функция отправки в порт пакета вида **$110 25 600 920;** из массива
	+ **Дополнительно** - несколько клёвых удобных функций
		+ medianFilter(a, b, c) - получить среднее из трёх (медианный фильтр)
		+ setPWMPrescaler(pin, prescaler) - установка частоты ШИМ для разных пинов (смотри пример)
		+ getVCC() - получить напряжение питания в милливольтах (мВ)
		+ getVoltage(pin) - получить напряжение на аналоговом пине с учётом реального питания (мВ)
		+ setConstant(voltage) - авто калибровка константы. В функцию подать напряжение питания в мВ (смотри пример)
		+ getTemp() - получить примерную температуру ядра
- **GyverRGB (текущая 1.3)** - удобное управление RGB светодиодом или лентой. Возможности:
	- Класс GRGB (r_pin, g_pin, g_pin) - настройка подключения
	- init() - инициализация
	- setRGB(R, G, B) - установка цвета в пространстве RGB. R, G, B принимают 0-255
    - setHSV(H, S, V) - установка цвета в пространстве HSV (цвет, насыщенность, яркость). Принимают 0-255
	- reverse(true) - установка реверса ШИМ для LED драйверов
	- setColor(color_hex) - установка цвета HEX кодом вида 0xFFFFFF
	- Несколько предустановленных цветов вида G_RED, G_PINK
- **GyverButton (текущая 1.1)** - библиотека для полной отработки нажатия кнопки. Возможности:
	+ Класс GButton (pin) - указать пин, куда подключена кнопка (PIN --- КНОПКА --- GND)
	+ setDebounce(time) - время антидребезга (по умолчанию 80 мс)
	+ setTimeout(time) - таймаут на удержание/повторное нажатие (по умолчанию 500 мс)
	+ tick() - опрос кнопки с программным антидребезгом контактов
	+ butt1.isPress(), butt1.isRelease(), butt1.isHolded(), butt1.isHold() - отработка нажатия, удерживания отпускания кнопки
	+ isSingle(), butt1.isDouble(), butt1.isTriple - отработка одиночного, двойного и тройного нажатия (вынесено отдельно)
	+ getClicks() - отработка любого количества нажатий кнопки (функция возвращает число нажатий)
	+ getIncr(value) - функция изменения значения переменной с заданным шагом и заданным интервалом по времени
	+ setIncrStep(step) - настройка инкремента, может быть отрицательным (по умолчанию 1)
	+ setIncrTimeout(500) - настрйока интервала инкремента (по умолчанию 800 мс)
	+ Пример использования в папке examples, показывает все возможности библиотеки
	+ Отличия от oneBtton и подобных библиотек: методы библиотеки не создают новые функции, что упрощает применение в сотни раз
- **GyverEncoder (текущая 1.1)** - библиотека для отработки энкодера. Возможности:
	+ Класс Encoder (CLK, DT, SW) - подключение пинов
	+ setCounters(norm, hold) - установка значений для инкремента
	+ setCounterNorm(norm), setCounterHold(hold) - то же самое, но отдельно
	+ setSteps(norm, hold) - установка шага изменения
	+ setStepNorm(norm), setStepHold(hold) - то же самое, но отдельно
	+ setLimitsNorm(normMin, normMax), setLimitsHold(holdMin, holdMax) - пределы изменения
	+ invert() - инвертировать направление
	+ tick() - отработка (опрос)
	+ isTurn(), isRight(), isLeft() - отработка поворота
	+ isRightH(), isLeftH() - отработка поворота с нажатием
	+ isPress(), isRelease(), isHolded(), isHold() - отработка нажатия кнопки с антидребезгом
	+ normCount, holdCount - получить значение со встроенного инкрементора
	+ Отработка "нажатого поворота" - такого вы нигде не найдёте. Расширяет возможности энкодера ровно в 2 раза
	+ Имеет встроенный счётчик для поворота, всё можно настроить
	+ Настраиваемые пределы изменяемой величины, а также шаг изменения
	+ Пример использования в папке examples, показывает все возможности библиотеки
- **GyverRTOS (текущая 1.0)** - система реального времени для Arduino. Возможности:
	- Мы создаём несколько функций с разным периодом выполнения (задачи)
    - Настраиваем период пробуждения системы (минимально 15 мс)  
    Далее всё автоматически:
    - Рассчитывается время до выполнения самой "ближней" задачи
    - Система периодически просыпается и считает таймеры
    - При наступлении времени выполнения ближайшей задачи, она выполняется. После этого снова выполняется расчёт времени до новой ближайшей задачи
    - Как итог: Ардуино спит (в зависимости от периодов) 99.999% времени, просыпаясь только для проверки флага и расчёта таймера
	- Смотри пример
- **TM74HC595_Gyver (текущая 1.1)** - библиотека для дисплея на сдвиговике TM74HC595
	+ Подробное описание здесь http://alexgyver.ru/tm74hc595_display/
	+ Пример использования в папке examples, показывает все возможности библиотеки
- **TM1637_Gyver (текущая 1.1)** - библиотека для дисплея на сдвиговике TM1637
	+ Подробное описание здесь http://alexgyver.ru/tm1637_display/
	+ Пример использования в папке examples, показывает все возможности библиотеки