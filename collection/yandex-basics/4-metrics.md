## Итоговый Конспект: Метрики Качества в Машинном Обучении

### 1. Введение: Зачем Нужны Метрики?

* **Определение:** Метрика качества – это количественная мера, позволяющая оценить, насколько хорошо модель машинного обучения справляется с поставленной задачей (классификацией, регрессией и т.д.) на определенном наборе данных.
* **Цель:** Объективно сравнить разные модели или разные версии одной модели, выбрать наилучшую, диагностировать проблемы (например, переобучение) и понять, удовлетворяет ли модель бизнес-требованиям.
* **Контекст Важен:** Выбор метрики(метрик) **критически зависит** от конкретной задачи, свойств данных (например, баланса классов) и бизнес-целей (цены разных типов ошибок).

### 2. Основа для Метрик Классификации: Матрица Ошибок (Confusion Matrix)

* **Определение:** Таблица, которая визуализирует производительность алгоритма классификации. Сравнивает предсказанные классы с истинными классами. Для бинарной классификации (классы 0 и 1) выглядит так:

    |                    | Предсказано: 1 (Positive) | Предсказано: 0 (Negative) |
    | :----------------- | :------------------------ | :------------------------ |
    | **Истина: 1 (Pos)** | **TP** (True Positive)    | **FN** (False Negative)   |
    | **Истина: 0 (Neg)** | **FP** (False Positive)   | **TN** (True Negative)    |

* **Обозначения:**
  * **TP (True Positive / Истинно Положительный):** Объекты класса 1, правильно предсказанные как класс 1.
  * **TN (True Negative / Истинно Отрицательный):** Объекты класса 0, правильно предсказанные как класс 0.
  * **FP (False Positive / Ложно Положительный / Ошибка I рода):** Объекты класса 0, ошибочно предсказанные как класс 1. (Пример: здоровому поставили диагноз "болен").
  * **FN (False Negative / Ложно Отрицательный / Ошибка II рода):** Объекты класса 1, ошибочно предсказанные как класс 0. (Пример: больного пропустили, сказав "здоров").
* **Зачем нужна:** Все основные метрики бинарной классификации (Accuracy, Precision, Recall, F1 и др.) рассчитываются на основе значений TP, TN, FP, FN.

### 3. Метрики Регрессии

* **Задача:** Оценить, насколько близки предсказанные непрерывные значения $\hat{y}^{(i)}$ к истинным $y^{(i)}$.
* **Обозначения:**
  * $m$ - количество примеров в выборке.
  * $y^{(i)}$ - истинное значение для $i$-го примера.
  * $\hat{y}^{(i)}$ - предсказанное значение для $i$-го примера.
  * $\bar{y} = \frac{1}{m} \sum_{i=1}^{m} y^{(i)}$ - среднее истинных значений.
* **Основные Метрики:**
  * **MSE (Mean Squared Error / Среднеквадратичная Ошибка):**
    * Формула: $\text{MSE} = \frac{1}{m} \sum_{i=1}^{m} (y^{(i)} - \hat{y}^{(i)})^2$
    * Смысл: Средний квадрат разности между истинным и предсказанным значениями.
    * Свойства: Сильно штрафует за большие ошибки (из-за квадрата). Чувствительна к выбросам. Имеет размерность квадрата целевой переменной.
  * **RMSE (Root Mean Squared Error / Корень из Среднеквадратичной Ошибки):**
    * Формула: $\text{RMSE} = \sqrt{\text{MSE}} = \sqrt{\frac{1}{m} \sum_{i=1}^{m} (y^{(i)} - \hat{y}^{(i)})^2}$
    * Смысл: Корень из MSE.
    * Свойства: Имеет ту же размерность, что и целевая переменная, что делает ее более интерпретируемой, чем MSE. Также чувствительна к выбросам и большим ошибкам.
  * **MAE (Mean Absolute Error / Средняя Абсолютная Ошибка):**
    * Формула: $\text{MAE} = \frac{1}{m} \sum_{i=1}^{m} |y^{(i)} - \hat{y}^{(i)}|$
    * Смысл: Средний модуль разности между истинным и предсказанным значениями.
    * Свойства: Менее чувствительна к выбросам, чем MSE/RMSE. Имеет ту же размерность, что и целевая переменная.
  * **R² (Coefficient of Determination / Коэффициент Детерминации):**
    * Формула: $R^2 = 1 - \frac{\sum_{i=1}^{m} (y^{(i)} - \hat{y}^{(i)})^2}{\sum_{i=1}^{m} (y^{(i)} - \bar{y})^2} = 1 - \frac{RSS}{TSS}$
      * $RSS = \sum (y^{(i)} - \hat{y}^{(i)})^2$ (Residual Sum of Squares - Сумма Квадратов Остатков)
      * $TSS = \sum (y^{(i)} - \bar{y})^2$ (Total Sum of Squares - Общая Сумма Квадратов)
    * Смысл: Доля дисперсии целевой переменной, объясненная моделью. Показывает, насколько модель лучше, чем простое предсказание среднего $\bar{y}$.
    * Свойства: Диапазон от $-\infty$ до 1. Ближе к 1 - лучше. $R^2=0$ - модель не лучше среднего. $R^2<0$ - модель хуже среднего. **Неубывание при добавлении признаков** делает его неидеальным для сравнения моделей разной сложности.
  * **Adjusted R² (Скорректированный R²):**
    * Формула: $R^2_{adj} = 1 - (1 - R^2) \frac{m-1}{m-n-1}$ (где $n$ - число признаков).
    * Смысл: Модификация R², которая штрафует за добавление бесполезных признаков.
    * Свойства: Всегда $\le R^2$. Используется для сравнения моделей с разным числом признаков.
  * **MAPE (Mean Absolute Percentage Error / Средняя Абсолютная Процентная Ошибка):**
    * Формула: $\text{MAPE} = \frac{100\%}{m} \sum_{i=1}^{m} |\frac{y^{(i)} - \hat{y}^{(i)}}{y^{(i)}}|$ (Осторожно: не определена при $y^{(i)}=0$).
    * Смысл: Средняя ошибка в процентах от истинного значения.
    * Свойства: Интуитивно понятна, но несимметрична и нестабильна при малых $y^{(i)}$.

### 4. Метрики Бинарной Классификации (Пороговые)

* Эти метрики зависят от выбора **порога (threshold)**, который переводит предсказанные вероятности $\hat{p}^{(i)}$ в классы (0 или 1). Стандартный порог = 0.5.
* **Accuracy (Доля Правильных Ответов):**
  * Формула: $\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN} = \frac{\text{Верные предсказания}}{\text{Все предсказания}}$
  * Смысл: Доля объектов, классифицированных правильно.
  * **Проблема:** **Очень обманчива при дисбалансе классов.** (См. твой пример про 99 здоровых). Не показывает, ошибки какого рода совершает модель. **Красный флаг, если кандидат полагается только на Accuracy при дисбалансе.**
* **Precision (Точность):**
  * Формула: $\text{Precision} = \frac{TP}{TP + FP}$
  * Смысл: Какая доля объектов, которые модель назвала положительными (класс 1), *действительно* являются положительными? Насколько можно "доверять" предсказаниям класса 1.
  * Фокус: Минимизация **FP (Ложных Срабатываний)**.
  * Пример Задачи: Спам-фильтр (важнее не отправить важное письмо в спам (FP), чем пропустить спам (FN)). Поиск (важнее выдать релевантные результаты (TP) и мало нерелевантных (FP)).
* **Recall (Полнота / Sensitivity / True Positive Rate, TPR):**
  * Формула: $\text{Recall} = \frac{TP}{TP + FN}$
  * Смысл: Какую долю *всех реальных* положительных объектов (класс 1) модель смогла обнаружить?
  * Фокус: Минимизация **FN (Пропусков Положительного Класса)**.
  * Пример Задачи: Медицинская диагностика (важнее обнаружить болезнь (TP) и не пропустить ее (FN), даже если будут ложные тревоги (FP)). Обнаружение мошенничества.
* **Trade-off Precision и Recall:** Обычно улучшение одной из этих метрик (при фиксированной модели, меняя порог) ведет к ухудшению другой. Нельзя максимизировать обе одновременно без ограничений.
* **F1-Score (F1-Мера):**
  * Формула: $F1 = 2 \cdot \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$
  * Смысл: **Гармоническое среднее** Precision и Recall. Используется для получения единой метрики, которая балансирует обе.
  * **Почему Гармоническое:** Оно ближе к меньшему из двух значений и сильно штрафует, если одна из метрик (P или R) очень низкая. Арифметическое среднее $(P+R)/2$ может быть высоким, даже если одна метрика почти нулевая. F1 будет высоким только если *обе* метрики достаточно высоки.
  * Применение: Хороший выбор при дисбалансе классов или когда важны и Precision, и Recall.
* **F-beta Score (Обобщенная F-Мера):**
  * Формула: $F_\beta = (1 + \beta^2) \frac{\text{Precision} \times \text{Recall}}{(\beta^2 \times \text{Precision}) + \text{Recall}}$
  * Смысл: Обобщение F1, позволяющее придать больший вес либо Precision, либо Recall с помощью параметра $\beta \ge 0$.
  * Интерпретация $\beta$:
    * $\beta = 1$: F1-Score (равный вес).
    * $\beta > 1$: Recall важнее Precision в $\beta$ раз. (Пример: $\beta=2$ значит Recall в 2 раза важнее).
    * $0 \le \beta < 1$: Precision важнее Recall. (Пример: $\beta=0.5$ значит Precision в 2 раза важнее).

### 5. Метрики Бинарной Классификации (Ранжирующие / Порогонезависимые)

* Эти метрики оценивают **качество ранжирования** объектов моделью (насколько хорошо она отделяет класс 1 от класса 0 по предсказанным вероятностям $\hat{p}$), **независимо от выбора конкретного порога**.
* **ROC-Кривая (Receiver Operating Characteristic Curve):**
  * **Определение:** График, показывающий зависимость **TPR (Recall)** от **FPR** при изменении порога классификации.
  * **Оси:**
    * Ось Y: **True Positive Rate (TPR)** = Recall = $TP / (TP + FN)$ (Доля найденных позитивных).
    * Ось X: **False Positive Rate (FPR)** = $FP / (FP + TN)$ (Доля негативных, ошибочно названных позитивными).
  * **Построение:** Варьируем порог от 1 до 0. Для каждого порога считаем TPR и FPR, ставим точку на графике. Соединяем точки.
  * **Интерпретация Кривой:**
    * Идеальная модель: Точка (0, 1) (TPR=1, FPR=0). Кривая идет из (0,0) в (0,1), затем в (1,1).
    * Случайный классификатор: Диагональная линия из (0,0) в (1,1).
    * Хорошая модель: Кривая стремится к левому верхнему углу. Чем выше и левее кривая, тем лучше модель разделяет классы.
* **ROC AUC (Area Under the ROC Curve / Площадь под ROC-кривой):**
  * **Определение:** Площадь под ROC-кривой.
  * **Диапазон:** От 0 до 1.
  * **Интерпретация Значения:**
    * AUC = 1: Идеальный классификатор.
    * AUC = 0.5: Случайный классификатор (нет разделяющей способности).
    * AUC < 0.5: Классификатор работает хуже случайного (возможно, перепутаны метки классов).
    * AUC > 0.5: Есть разделяющая способность. Чем ближе к 1, тем лучше.
    * **Вероятностный Смысл:** AUC равен вероятности того, что случайно выбранный объект класса 1 будет иметь предсказанную вероятность $\hat{p}$ **выше**, чем случайно выбранный объект класса 0. Оценивает качество **ранжирования**.
* **PR-Кривая (Precision-Recall Curve):**
  * **Определение:** График, показывающий зависимость **Precision** от **Recall** при изменении порога классификации.
  * **Оси:**
    * Ось Y: **Precision** = $TP / (TP + FP)$
    * Ось X: **Recall** = $TP / (TP + FN)$
  * **Построение:** Аналогично ROC, варьируем порог, для каждого считаем Precision и Recall, ставим точку, соединяем.
  * **Интерпретация Кривой:**
    * Идеальная модель: Точка (1, 1) (Recall=1, Precision=1). Кривая идет из (0, Precision при Recall=0) в (1,1).
    * **Случайный классификатор:** **Горизонтальная линия** на уровне $y = P / (P+N) = \text{Доля позитивного класса}$. (Важно: Не диагональ!).
    * Хорошая модель: Кривая стремится к правому верхнему углу.
* **PR AUC (Area Under the PR Curve / Площадь под PR-кривой):**
  * **Определение:** Площадь под PR-кривой.
  * **Диапазон:** От (Доля позитивного класса) до 1.
  * **Интерпретация:** Оценивает способность модели находить позитивные объекты (высокий Recall) с высокой точностью (высокий Precision).

### 6. ROC AUC vs PR AUC: Что Выбрать при Дисбалансе Классов?

* **Ключевой Вопрос Интервью:** Какая метрика лучше при дисбалансе? **Ответ:** Зависит от цели, но **PR AUC часто более информативна**, если важен **минорный (позитивный) класс**.
* **Почему ROC AUC может быть обманчив:**
  * Ось X - FPR = $FP / (FP + TN)$.
  * При сильном дисбалансе, количество негативных объектов $TN$ **очень велико**.
  * Даже если модель генерирует много ложных срабатываний $FP$ (что плохо для Precision), их число может быть мало по сравнению с $TN$.
  * Поэтому $FPR$ может оставаться низким, и ROC-кривая (и ROC AUC) будет выглядеть хорошо, **скрывая** проблему большого числа $FP$ и низкой Precision.
* **Почему PR AUC более чувствителен (информативен):**
  * Ось Y - Precision = $TP / (TP + FP)$.
  * Знаменатель Precision напрямую зависит от $FP$.
  * При дисбалансе, даже небольшое (в абсолютном выражении) количество $FP$ может быть сравнимо или больше, чем $TP$ (которых мало).
  * Любое увеличение $FP$ **сильно снижает** Precision и "опускает" PR-кривую.
  * PR-кривая и PR AUC **явно показывают** способность модели находить редкий позитивный класс с приемлемой точностью.
* **Вывод:**
  * **ROC AUC:** Хорош для оценки общей **ранжирующей способности** и когда баланс классов примерно равен или когда важны оба класса одинаково.
  * **PR AUC:** **Предпочтителен при сильном дисбалансе и фокусе на производительности модели именно на минорном (позитивном) классе.**

### 7. Выбор Метрики: Общие Рекомендации

* **Нет "Лучшей" Метрики:** Выбор всегда зависит от **контекста задачи**.
* **Задайте Вопросы:**
  * Какая цель модели? (Найти всех больных? Не ошибиться в диагнозе?)
  * Какова цена ошибок I и II рода (FP и FN)?
  * Есть ли дисбаланс классов? Насколько он сильный?
  * Нужно ли интерпретировать результат как вероятность или достаточно ранжирования?
* **Рекомендации:**
  * **Сбалансированные классы:** Accuracy (как базовая), F1, ROC AUC.
  * **Дисбаланс, важен позитивный класс:** Precision, Recall, F1/F-beta, **PR AUC**. ROC AUC – с осторожностью, смотреть вместе с другими.
  * **Нужно оценить ранжирование:** ROC AUC, PR AUC.
  * **Регрессия:** RMSE/MAE (для оценки ошибки в единицах таргета), R²/Adjusted R² (для доли объясненной дисперсии), MAPE (если важна процентная ошибка).

### 8. Статистическая Значимость Метрик

* **Важно Помнить:** Любая метрика, посчитанная на конечной (тестовой) выборке, является **случайной величиной**. Она лишь *оценка* истинного качества модели.
* **Проблема:** Улучшение метрики (например, AUC с 0.85 до 0.86) может быть случайным из-за особенностей тестовой выборки.
* **Решение:** Использовать методы статистической проверки гипотез (например, парные тесты для результатов CV) или Bootstrap для оценки доверительных интервалов и понимания, является ли разница между моделями **статистически значимой**. (Это тема для отдельного глубокого разбора, но важно упомянуть о ней в контексте метрик).
