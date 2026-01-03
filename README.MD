# DOCS

## Feature Sliced Design (FSD)

- [official docs](https://feature-sliced.design/docs)

### Доклады

- про FSD [Feature-Sliced Design / Александр Моргунов](https://youtu.be/M84x3pzDYr0)
- FSD на примере MVP проектов (3-6 месяцев) на практике [Эволюция архитектуры. Как мы пришли к FSD / Сергей Пономарёв, Purrweb](https://youtu.be/KVcY1bfUCBU)
- docs

### Examples

#### Official react examples

- [nukeapp](https://github.com/noveogroup-amorgunov/nukeapp)
- [realworld-react-fsd](https://github.com/yurisldk/realworld-react-fsd)

#### FSD MWWM MOBX example

- [fsd_mwwm_mobx_demo](https://github.com/altynbek132/fsd_mwwm_mobx_demo)

### Community

- [tg community](https://t.me/feature_sliced)
- [use as community discussions reference bot, do not rely on his answers](https://t.me/feature_sliced/92691)

### Введение

После анализа различных докладов становится очевидным, что команды разработки часто приходят к организации кодовой базы по фичам. Традиционная Чистая Архитектура предусматривает разделение только по слоям, чего не всегда достаточно для поддержания чистой кодовой базы с низкой связанностью. Поэтому вводится вертикальное разделение по фичам, где слои представляют горизонтальные срезы из Чистой Архитектуры, а фичи - вертикальные.

### Соответствие слоев c чистой Архитектурой

![диаграмма](https://github.com/altynbek132/arbihunter_docs/blob/main/clean_fsd.png)

- "Domain" → "Entities"
- "Application" → "Features"

#### Различие

![диаграмма](https://github.com/altynbek132/arbihunter_docs/blob/main/clean_fsd_ui.png)

- UI выделяется как отдельный слой во внешнем слое

### Основные преимущества

- UI распределен по слоям (что удобно, так как UI-элементы часто связаны с сущностями, фичами или виджетами)
- Четкое разделение по фичам и слоям
- Строгий порядок импортов (верхние слои могут импортировать из нижних, но не наоборот)
- Слайсовая организация каждого слоя

### Ключевые пояснения

#### Слой Features

- Переиспользуемая бизнес-логика

#### Слайсы

- Группы связанных модулей
- Высокая связность внутри
- Отсутствие кросс-импортов

### Предлагаемые дополнения

1. Опциональные под-слайсы
2. Гибкие правила кросс-импортов для виджетов
3. Применение паттернов в подслое фич с агрегацией фич

### Рекомендации по разработке

1. Начало с:
   - pages
   - shared компонентов
2. Размещение логики:
   - Изначально в слое pages в widget_model рядом с экраном
   - Вынос в отдельные сущности/слайсы при переиспользовании 3+ раз

### Заключение

Такой подход обеспечивает лучшую организацию, сохраняя при этом гибкость и масштабируемость.

---

Если вы хотите задавать вопросы по определенной документации (например, FSD) и получать ответы с референсами, вы можете:

Скачать весь репозиторий с документацией

Загрузить файлы в формате Markdown в:

- Gemini
- или Claude (если хватает размера контекстного окна)

После этого AI сможет отвечать на ваши вопросы, опираясь на загруженную документацию и предоставлять соответствующие ссылки на источники.

[пример](https://github.com/altynbek132/arbihunter_docs/blob/main/msty_example.png)

## MOBX + STREAM

- [official docs](https://mobx.netlify.app/)

Я решил полностью отказаться от кодогенерации при работе с MobX по двум причинам.

- сам процесс кодогенерации может быть проблематичным.
- сгенерированный код часто сложен для понимания.

Использование MobX напрямую, без кодогенерации, делает код более прозрачным и понятным - легче отследить, как и что работает.

- [mobx_utils.dart](https://github.com/altynbek132/flutter_util_packages/blob/main/packages/utils/lib/utils_dart/mobx_utils.dart)
  - MobXUtilsDart - это вспомогательный класс, который предоставляет утилиты для конвертации MobX Observable объектов в Stream-ы, а также для их создания.
- [mobx_utils.dart](https://github.com/altynbek132/flutter_util_packages/blob/main/packages/utils/lib/utils_flutter/mobx_utils.dart)
  - MobXUtils содержит методы для преобразования Listenable объектов из Flutter в Observable, поскольку по умолчанию Listenable, который отправляет уведомления об изменениях, не вызывает реакции MobX.
- [mobx_store_base.dart](https://github.com/altynbek132/flutter_util_packages/blob/main/packages/utils/lib/utils_flutter/mobx_store_base.dart)
  - Этот абстрактный класс служит основой для создания виджет-модуля, интегрированного с MobX. В нём реализованы асинхронный и синхронный методы dispose, а также механизм логирования. Класс дополнен миксином disposable bag, позволяющим использовать весь виджет-модуль в качестве disposable bag и присоединять к нему различные disposables-объекты.

## ELEMENTARY

- [official docs](https://pub.dev/packages/elementary)
- [vscode mwwm+mobx generator](https://marketplace.visualstudio.com/items?itemName=altynbek132.mobx-elementary-generator)
  - здесь нужно правой кнопкой мыши нажать на папку и выбрать пункт "Generate" (или "Сгенерировать")
- [Elementary — MVVM для Flutter под новым углом / Михаил Зотьев (Surf)](https://www.youtube.com/watch?v=pehjIuuvPUQ)

Давайте разберем суть Elementary и его роль в разработке:

Elementary решает важную архитектурную задачу - разделение пользовательского интерфейса (UI) от логики UI и бизнес-логики. Это классическая реализация паттерна MVVM (Model-View-ViewModel).

Я создал модифицированную версию ([форк](https://github.com/altynbek132/flutter-elementary)) Elementary, которая делает разделение бизнес-логики опциональным. Это позволяет:

- Ускорить процесс разработки
- При необходимости объединять в widget-model как UI логику, так и бизнес-логику
- Идти на компромисс между скоростью разработки и техническим долгом

Важно отметить, что концепция ViewModel, используемая в Elementary, берет свое начало из Android-разработки. В обеих платформах ViewModel выполняет схожие функции:

- Управление состоянием UI
- Обработка взаимодействия с бизнес-логикой
- Координация между представлением и данными

Таким образом, и Android ViewModel, и widget-model в Elementary концептуально очень близки, так как решают одни и те же архитектурные задачи.

## SYNCHRONIZED

- [official docs](https://pub.dev/packages/synchronized)

### Решение проблемы Race Condition с помощью пакета Synchronizer

#### Проблема

В UI-системах существует проблема race condition при обработке событий. Поскольку все выполняется в Event Loop, пользователь может:

- Одновременно нажать несколько кнопок
- Быстро нажать одну кнопку несколько раз

#### Два основных подхода решения

##### 1. Использование Streams с AsyncMap

- События обрабатываются через AsyncMap
- Следующее событие не начнет обрабатываться до завершения текущего
- Обработка происходит последовательно

##### 2. Использование пакета Synchronizer с locks

- Создается mutex-подобный механизм
- Вызывается метод synchronize с асинхронной функцией в качестве аргумента
- Lock гарантирует, что новая асинхронная функция не запустится до завершения предыдущей
- Обеспечивает последовательное выполнение операций

Оба метода помогают избежать конфликтов при параллельной обработке событий, обеспечивая их упорядоченное выполнение.

### SYNCHRONIZED+MOBX

- [observable_lock](https://github.com/altynbek132/flutter_util_packages/blob/main/packages/utils/lib/utils_dart/observable_lock.dart)
  - Я реализовал обёртку над механизмом блокировки, обеспечивающую реактивное отслеживание состояния через MobX.
