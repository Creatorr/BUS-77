# Быстрый старт с использованием SDK

iRidium SDK представляет собой реализацию децентрализованной части протокола в виде набора исходных файлов. Реализация выполнена в виде класса, который содержит собственные методы для реализации работы протокола и методы обратного вызова, которые реализуют особенности работы устройства.

## Как начать работу?

Подготовка аппаратной части (по минимуму) потребуется:
* Два микроконтроллера stm32f103c8t6 (первый в качестве преобразователя из Serial в CAN, а второй как тестовое устройство).
* Два CAN трансивера (например: SN65HVD230D маркируется как VP230)
* USB-TTL преобразователь.

Настройка инструментов разработки.
* Скачать и установить среду разработки Keil для ARM с официального сайта https://www.keil.com. В этой среде будет производится компиляция проекта.
* Скачать и установить STM32CubeMX с официального сайта https://www.st.com/en/development-tools/stm32cubemx.html. С помощью данного инструмента будет производиться настройка проекта.
* Скачать iRidium SDK с github по ссылке https://github.com/iRidiumMobileLTD/BUS-77. 

Изготовление шлюза для программирования шины.
* Собрать [[Bus77_USB|схему шлюза]].
* Открыть и скомпилировать программу шлюза, файл проект находится Example\STM32\STM32F103C8T6\GateUARTtoCAN\MDK-ARM\GateUARTtoCAN.uvprojx

Изготовление конечного устройства.
* Собрать схему конечного устройства.
* Сборка проекта и загрузка проекта на микроконтроллер.

  a) С использованием шаблона:
  
  * Для этого надо открыть и скомпилировать загрузчик, файл проекта находится Example\STM32\STM32F103C8T6\Template\Bootloader\MDK-ARM\Bootloader.uvprojx
  *	С помощью Keil загрузить загрузчик на микроконтроллер.
   *	Для этого надо открыть и скомпилировать прошивку, файл проекта находится Example\STM32\STM32F103C8T6\Template\Firmware\MDK-ARM\Firmware.uvprojx
  *	С помощью утилиты config.exe (находится в директории Utility) загрузить созданную прошивку на микроконтроллер.
  
  b)	Без использования шаблона.
  
  *	Создать класс-наследник от класса CIridiumBusProtocol, который будет реализовывать функционал вашего устройства.
  * В конструкторе вашего класса нужно:
    * Инициализировать входящий буфер для приема пакетов протокола.
    * Инициализировать исходящий буфер для отправки пакетов протокола.
  * Реализовать обработчики системных методов:
    * Обработчик получения информации об устройстве при получении поискового запроса.
    * Обработчик изменения локального идентификатора.
    * Получение информации об устройстве.
  * Реализовать обработчики работы с глобальными переменными:
    * Обработчик установки значения глобальной переменной.
    * Обработчик получения значения глобальной переменной.
  * Реализовать обработчики для работы с каналами обратной связи.
    * Обработчик получения количества каналов обратной связи.
    * Обработчик преобразования идентификатора канала обратной связи в индекс канала обратной связи.
    * Обработчик получения данных канала обратной связи.
    * Обработчик получения расширенного описания канала обратной связи.
    * Обработчик связывания глобальной переменной с каналом обратной связи.
    * Установка значения канала обратной связи.
  * Реализовать обработчики для работы с каналами управления.
    * Обработчик получения количества каналов управления.
    * Обработчик преобразования идентификатора канала управления в индекс канала управления.
    * Обработчик получения данных канала управления.
    * Обработчик получения расширенного описания канала управления.
    * Обработчик связывания списка глобальных переменных с каналом управления.
    * Установка значения канала управления.
  * Реализовать работу с потоками (Потоки используются для перепрошивки устройств)
    * Обработчик открытия потока.
    * Обработчик подтверждения открытия потока.
    * Обработчик отправки блока.
    * Обработчик подтверждения получения блока.
    * Обработчик закрытия потока.
  * Реализовать обработчик  Smart API
  * После компиляции с помощью среды разработки keil загрузить созданную прошивку на микроконтроллер.