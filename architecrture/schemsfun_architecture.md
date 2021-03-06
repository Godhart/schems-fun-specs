# Control

Оркестрирует работу всех компонентов.

Идея - изолировать управление от графического интерфейса для возможности авотматизации проверок, а так же для дистанционнной работы.

Несёт в себе три функции:

- управление режимом работы
- машина состояний для выполнения последовательных действий
- распределение запросов на связанные компоненты

## Mode

Выбор режима управления / взаимодействия.

Влияет на:

* обработку кликов и другого взаимодействия на рабочей зоне
* отрисовку элементов

## FSM

Определяет возможные действия на основании режима и предыдущих действий.

Так же содержит все состояния при работе со схемой - список выбранных элементов и т.п.

## Dispatch

Изолирует экземпляры компонентов друг от друга, так чтобы им приходилось взаимодействовать всего с одним компонентом (там где нет требований на производительность).

Принимает вызов, переадресует его нужному компоненту, возвращает результат.

Варианты взаимодействия - синхронное и асинхронное (с вызовом функции по завершению).

Во время приёма вызова - учитывает текущий режим и состояние FSM чтобы предотвратить некорректное взаимодействие.

# UI

Графический интерфейс. Содержит:

* **Work Area** для отображения схемы и непосредственного взаимодействия с ней
* Элементы интерфейса - панели, кнопки и пр. для выполнения прочих действий
* Обеспечивает компоновку элементов интерфейса
* Обеспечивает отображение текста на разных языках

Обеспечивает режим Picture-in-Picture для совмещения основной схемы и дополнительной информации

# Touch Logic

Абстрагирует платформо-зависимый интерфейс взаимодействия в единый интерфейс

# Interactive Logic

Принимает интерактивные события и реагирует на них в соответствии с текущим режимом и состоянием FSM.

Принимает события интерактивного взаимодействия (клики, перемещение курсора, жесты, кнопки) и обрабатывает их в зависимости от режима:

* определяет объект взамодействия (по координатам события)  (через Renderer)
* определяет тип взаимодействия
* вызывает функции FSM компонента Control в соответствии с режимом/объектом/взаимодействием
* вызывает интерактивные события или меню выбора интерактивных событий, если возмжно несколько вариантов

# Work Area

Содержит:

* канвы для рисования (несколько слоёв для разных целей)

* области размещения объектов (виджетов) (несколько слоёв для разных целей)
* область для захвата интерактивных действий

Используется Rendering Engine для отображения схемы и дополнительнй индикации (канва и область размещения объектов)

Передаёт события интеактивных действий на Interactive Logic

# Rendering Engine

Содержит объекты для отображения элементов схемы. Объекты в т.ч. имеют дианимческую реакцию на изменение свойств элементов схемы (текущее состояние и значение сигналов) 

Формирует на основе описания схемы объекты для отображения элементов

Обеспечивает отрисовку элементов схемы на **Work Area** через элементы абстракции интерфейса (Renderer_Proxy)

Обеспечивает дополнительную отрисовку графики на схеме на основании текущего режима, состония FSM и самой схемы

#  Schematics Engine

Обеспечивает действия со схемой:

- добавление/удаление/изменение компонентов
- добавление/удаление/изменение связей
- операции с различными представлениями схемы
- операции undo/redo
- сохранение/загрузку схемы
- импорт схемы из внешнего источника:
  - отображение изменений
  - синхронизация
  - отслеживание сохранённых отличий
  - синхронизация в режиме whiteboard/trainer/colab
- иерархическое отображение схем ("раскрывать" сложный элемент в родительской схеме, рекурсивно)

# Storage Engine

Хранение локальных данных:

* схемы
* словари перевода (кеш)
* библиотеки (кеш)
* синхронизация с внешним хранилищем

Хранение данных во внешней базе:

* схемы
* публикация схем (слепок)
* публикация историй
* кастомные отображения
* кастомные элементы
* кастомный перевод
* управление доступом

Синхронизация локальных и внешних данных в режиме whiteboard/trainer/colab 

