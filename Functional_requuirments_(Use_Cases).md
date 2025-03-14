## Use Case: Инициализация холста
### Актер: Система
### Предусловия: Отсутствуют
#### Основной сценарий успеха:
При запуске программы система создает объект Canvas с заданными шириной (Width) и высотой (Height).
Система инициализирует двумерный массив символов (char[,] CanvasArray) с размерами, соответствующими ширине и высоте.
Система заполняет весь массив пробелами (' ') с использованием вложенных циклов.
Система устанавливает текущий фоновый символ как пробел (CurrentBackgroundColor = ' ').
### Постусловия: Пустой холст фиксированного размера готов к использованию.

## Use Case: Рисование фигуры
### Актер: Пользователь
### Предусловия: Холст инициализирован.
#### Основной сценарий успеха:
Система предлагает пользователю выбрать тип фигуры: прямоугольник, круг или равнобедренный треугольник.
Пользователь выбирает тип фигуры.
Система запрашивает у пользователя параметры для выбранной фигуры:
##### Прямоугольник: 
Координаты верхней левой точки (X, Y), ширина (Width), высота (Height) и символ (Color).
##### Круг: 
Координаты центра (X, Y), радиус (Radius) и символ (Color).
##### Равнобедренный треугольник: 
Координаты вершины (X, Y), высота (Height) и символ (Color).
Пользователь вводит параметры через консоль.
Система проверяет корректность ввода (например, целые числа для координат и размеров, один символ для цвета).
Система создает объект фигуры и вызывает её метод Draw для заполнения массива CanvasArray указанным символом:
Для прямоугольника: заполняются точки в заданных пределах с проверкой границ холста.
Для круга: заполняются точки, удовлетворяющие уравнению (i - Y)^2 + (j - X)^2 <= Radius^2, с обрезкой за пределами холста.
Для треугольника: заполняются строки с шириной основания 2 * (i - Y + 1) - 1, с обрезкой за пределами холста.
Система добавляет фигуру в список _shapes.
Система обновляет отображение холста в консоли.
Система сохраняет текущее состояние в истории через метод UpdateHistory.
#### Расширения:
Если ввод некорректен (например, нечисловые значения или строка вместо символа), система выводит сообщение об ошибке и запрашивает повторный ввод.
Если фигура частично или полностью выходит за пределы холста, отображаются только части внутри границ.

## Use Case: Стирание фигуры
### Актер: Пользователь
### Предусловия: Холст инициализирован и содержит хотя бы одну фигуру в списке _shapes.
#### Основной сценарий успеха:
Пользователь выбирает опцию "Стереть фигуру" из меню.
Система запрашивает начальные координаты фигуры (X, Y).
Пользователь вводит координаты через консоль.
Система ищет в списке _shapes фигуру с совпадающими начальными координатами (верхняя левая точка для прямоугольника, центр для круга, вершина для треугольника).
Если фигура найдена, система удаляет её из списка _shapes.
Система очищает массив CanvasArray, заполняя его текущим фоновым символом (CurrentBackgroundColor).
Система перерисовывает все оставшиеся фигуры из списка _shapes на холсте, вызывая их методы Draw.
Система обновляет отображение холста в консоли.
Система сохраняет новое состояние в истории через UpdateHistory.
#### Расширения:
Если фигура с указанными координатами не найдена, система выводит сообщение: "Фигура с указанными координатами не найдена. Нажмите любую клавишу..." и завершает действие.
Если несколько фигур имеют одинаковые начальные координаты, система удаляет последнюю добавленную фигуру.

## Use Case: Перемещение фигуры
### Актер: Пользователь
### Предусловия: Холст инициализирован и содержит хотя бы одну фигуру в списке _shapes.
#### Основной сценарий успеха:
Пользователь выбирает опцию "Переместить фигуру" из меню.
Система запрашивает текущие начальные координаты фигуры (X, Y).
Пользователь вводит координаты через консоль.
Система ищет фигуру в списке _shapes с совпадающими начальными координатами.
Пользователь вводит новые координаты.
Система обновляет координаты найденной фигуры на новые значения.
Система очищает массив CanvasArray, заполняя его текущим фоновым символом.
Система перерисовывает все фигуры из списка _shapes, включая перемещенную, на холсте.
Система обновляет отображение холста в консоли.
Система сохраняет новое состояние в истории через UpdateHistory.
#### Расширения:
Если фигура с указанными координатами не найдена, система выводит сообщение: "Фигура с указанными координатами не найдена" и завершает действие.
Если новые координаты размещают фигуру частично или полностью за пределами холста, отображаются только части внутри границ.

## Use Case: Установка фона
### Актер: Пользователь
### Предусловия: Холст инициализирован.
#### Основной сценарий успеха:
Пользователь выбирает опцию "Добавить фон" из меню.
Система запрашивает новый символ для фона.
Пользователь вводит символ через консоль.
Система проверяет, что введен один символ.
Система обновляет значение CurrentBackgroundColor на новый символ.
Система очищает массив CanvasArray, заполняя его новым фоновым символом.
Система перерисовывает все фигуры из списка _shapes на холсте.
Система обновляет отображение холста в консоли.
Система сохраняет новое состояние в истории через UpdateHistory.
#### Расширения:
Если пользователь ввел более одного символа, система запрашивает повторный ввод.

## Use Case: Отмена действия (Undo)
### Актер: Пользователь
### Предусловия: Выполнено хотя бы одно действие, сохраненное в истории (History.Count > 1, CurrentStep > 0).
#### Основной сценарий успеха:
Пользователь выбирает опцию "Undo" из меню.
Система проверяет, доступно ли предыдущее состояние (CurrentStep > 0).
Система уменьшает значение CurrentStep на 1.
Система восстанавливает состояние холста (CanvasArray), списка фигур (_shapes) и фона (CurrentBackgroundColor) из соответствующих списков истории.
Система обновляет отображение холста в консоли.
#### Расширения:
Если предыдущее состояние отсутствует, завершает действие.

## Use Case: Возврат действия (Redo)
### Актер: Пользователь
### Предусловия: Отменено хотя бы одно действие (CurrentStep < History.Count - 1).
#### Основной сценарий успеха:
Пользователь выбирает опцию "Redo" из меню.
Система проверяет, доступно ли следующее состояние (CurrentStep < History.Count - 1).
Система увеличивает значение CurrentStep на 1.
Система восстанавливает состояние холста (CanvasArray), списка фигур (_shapes) и фона (CurrentBackgroundColor) из соответствующих списков истории.
Система обновляет отображение холста в консоли.
#### Расширения:
Если следующее состояние отсутствует, завершает действие.

## Use Case: Сохранение холста
### Актер: Пользователь
### Предусловия: Холст инициализирован.
#### Основной сценарий успеха:
Пользователь выбирает опцию "Сохранить холст" из меню.
Система запрашивает имя файла.
Пользователь вводит имя файла через консоль.
Система сериализует текущее состояние (CurrentBackgroundColor и список _shapes) в формат JSON с указанием полных имен типов фигур.
Система записывает JSON-данные в файл с указанным именем.
#### Расширения:
Если запись невозможна (например, из-за недопустимого имени файла) завершает действие.

## Use Case: Загрузка холста
### Актер: Пользователь
### Предусловия: Существует сохраненный файл холста в формате JSON.
#### Основной сценарий успеха:
Пользователь выбирает опцию "Загрузить холст" из меню.
Система запрашивает имя файла.
Пользователь вводит имя файла через консоль.
Система считывает файл и десериализует JSON-данные в CurrentBackgroundColor и список _shapes.
Система очищает массив CanvasArray, заполняя его загруженным фоновым символом.
Система перерисовывает все фигуры из загруженного списка _shapes на холсте.
Система обновляет отображение холста в консоли.
Система сохраняет новое состояние в истории через UpdateHistory.
#### Расширения:
Если файл не найден или поврежден, оставляет холст без изменений.
Если загруженные фигуры выходят за пределы холста, отображаются только части внутри границ.

## Use Case: Очистка холста
### Актер: Пользователь
### Предусловия: Холст инициализирован.
#### Основной сценарий успеха:
Пользователь выбирает опцию "Очистить холст" из меню.
Система сбрасывает CurrentBackgroundColor до пробела (' ').
Система заполняет массив CanvasArray текущим фоновым символом (CurrentBackgroundColor).
Система очищает список _shapes.
Система обновляет отображение холста в консоли.
Система сохраняет новое состояние в истории через UpdateHistory.
#### Расширения: Отсутствуют

## Use Case: Отображение холста
### Актер: Система
### Предусловия: Холст инициализирован.
#### Основной сценарий успеха:
Система очищает консоль.
Система выводит содержимое массива CanvasArray построчно, отображая текущие фигуры и фоновый символ.
#### Постусловия: Пользователь видит текущее состояние холста в консоли.
#### Расширения: Отсутствуют
#### Примечание: Этот use case вызывается автоматически после большинства действий пользователя (рисование, стирание, перемещение и т.д.).

## Use Case: Завершение работы
### Актер: Пользователь
### Предусловия: Отсутствуют
#### Основной сценарий успеха:
Пользователь выбирает опцию "Выход" ('0') из меню.
Система завершает выполнение программы.
#### Расширения: Отсутствуют
## Примечания:
### Интерфейс пользователя: 
Текстовое меню с 11 опциями (рисование, стирание, перемещение, фон, undo/redo, сохранение/загрузка, очистка, выход) является точкой входа для большинства use case и реализовано в методе Main.
### Обработка ошибок: 
Некорректный ввод (например, нечисловые значения или строки вместо символов) проверяется через методы ReadInt и ReadChar с выводом сообщений об ошибках.
### Управление историей: 
История действий обновляется после каждого изменения состояния (рисование, стирание и т.д.) через метод UpdateHistory, клонируя массив CanvasArray и список _shapes.
### Ограничения: 
Фиксированный размер холста задается при создании и не изменяется. Фигуры, выходящие за границы, обрезаются методами Draw с проверкой через IsWithinCanvas.
