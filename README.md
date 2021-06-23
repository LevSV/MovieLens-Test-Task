## Введение
Я построил две коллаборативные и одну контентную модель, а также их комбинацию. Коллаборативная модель предсказывает какую оценку поставит пользователь фильму, тогда как контентная модель предсказывает похожие к заданному фильмы. Для гибридного подхода я взял сначала результаты работы коллаборативной модели и дополнил их результатом работы контентной модели. Также привел в конце идеи по дальнейшему улучшению.

## Выбор метрики
Для коллаборативной модели я выбрал метрику RMSE, поскольку она дает понятную человеку величину ошибки прогнозирования рейтинга фильма, а также сама по себе функция дифференциируема и может быть использована при обучении. В контентной модели я не стал ипользовать метрики и сейчас постараюсь объяснить почему. Основная идея контентной модели в том, что она рекомендует товары, похожие на данные, т.е. метрика должна считать на сколько товары похожи. Однако, похожесть фильмов - это величина условная и у разных людей есть свои взгляды на похожесть фильмов. 
### Что можно сделать/улучшить 
Во-первых, надо понимать, что основная метрика у бизнеса - прибыль. Для ее увеличения у разных подразделений стоят свои метрики, например число/рост/вовлеченность пользователей. И уже в рамках подразделений есть текущие метрики, например в среднем через сколько предложенных фильмов пользователь находит тот, который будет смотреть. Уже на этих метриках можно тестировать контентные модели, например через A/B тестирование. В рамках этой задачи нет четкой цели, также нет важных данных, например, какие фильмы пользователь не стал смотреть. Если бы что-то из этого было, то тогда была бы возможность придумать адекватную метрику для контентной модели.

## Разбиение на обучение и валидацию
Посколько датасет достаточно большой, для тренировочной выборки я выбрал 95%, а для тестовой 5%.
### Что можно сделать/улучшить 
Выбор пользователей в тренировочную и тестовую выборку производился случайно, поэтому не для каждого пользователя соотношение одинаково. Можно ввести более честное разбиение, чтобы каждый пользователь был с одинаковым распределением, т.е. 95% данных о каждом пользователе уходит в тренировочную выборку, а 5% в валидационную. Также можно ввести разбиение, учитывая время, т.е. заносить в валидационную выборку последние активности. 

## Статистическая значимость
Отличие от бейзлайна, в котором предсказывается самое популярное/среднее значение, очевидно. Статистическую значимость отличий двух методов посчитать не представляется возможным в связии с отсутсвием метрики у контентного подхода. 
### Что можно сделать/улучшить
Можно внедрить A/B тестирование и считать статистическую значимость различий тестовой и контрольной группы. При A/B тестировании также можно сравнивать два подхода. 

## Выводы
В рамках данных условий нельзя точно сказать, что работает лучше, однако можно отметить плюсы и минусы каждых подходов, а также можно сравнить 2 коллаборативные модели.
### Коллаборативный подход
Проще для восприятия, а также значительно более быстро работает, нежели контентный. Могут возникать проблемы с нехваткой оперативной памяти. Лучший результат показал SVD алгоритм, нежели нейронная сеть, однако я считаю последнюю более лучшим выбором, поскольку она работает быстрее(при условии наличия GPU), ее можно еще значительно улучшить или вовсе придумать новую архитектуру. Также можно придумать как использовать временные данные об оценках, которые давали пользователи. В SVD такой возможности нет. Т.е. нейронная сеть лучше, на мой взгляд, из-за своей гибкости.
### Контентный подход
Работает достаточно медленно и сложно определять метрики. Если работать только с контентным алгоритмом, то пользователь будет получать в рекомендации только похожие фильмы, что не есть хорошо. Однако вкупе с коллаборативным подходом может подсказать фильмы, если попался "уникальный" пользователь, непохожий на других.
### Что можно сделать/улучшить
1) Доработать гибридную модель, поскольку сейчас она очень сырая. Например, не учитываются уже просмотренные фильмы
2) Протестировать другие архитектуры нейронных сетей, например попробовать учитывать timestamp через LSTM
3) Попробовать в гибридной модели использовать сначала контентную модель, а затем коллаборативную 
