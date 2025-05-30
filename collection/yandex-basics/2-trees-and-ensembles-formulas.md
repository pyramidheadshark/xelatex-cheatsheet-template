# Детальный Вывод Формул: Деревья Решений и Ансамбли

*(Используем обозначения: $m$ - число объектов, $n$ - число признаков, $x^{(i)}$ - вектор признаков $i$-го объекта, $y^{(i)}$ - истинное значение для $i$-го объекта, $K$ - число классов (для классификации), $p_k$ - доля объектов класса $k$ в узле, $N$ или $N_{node}$ - число объектов в узле, $N_c$ - число объектов в дочернем узле $c$.)*

## Часть 1: Дерево Решений (Decision Tree)

### 1.1 Критерии "Чистоты" Узла (Impurity Measures) - Классификация

Критерии чистоты используются для оценки того, насколько хорошо сплит разделяет данные на однородные группы в дочерних узлах. Цель - найти сплит, максимизирующий уменьшение нечистоты (Information Gain).

#### 1.1.1 Gini Impurity (Индекс Джини)

**Определение:** Вероятность того, что случайно выбранный элемент из узла будет неправильно классифицирован, если ему присвоить случайный класс согласно распределению классов в этом узле.

**Формула:**
$$
G = \sum_{k=1}^{K} p_k (1 - p_k)
$$

* **$p_k$**: Доля объектов класса $k$ в текущем узле. Рассчитывается как $N_k / N_{node}$, где $N_k$ - число объектов класса $k$ в узле, $N_{node}$ - общее число объектов в узле.
* **$(1 - p_k)$**: Вероятность того, что объект *не* принадлежит классу $k$.
* **$p_k (1 - p_k)$**: Вероятность выбрать объект класса $k$ и ошибочно присвоить ему другой класс (согласно распределению). Суммирование по всем классам дает общую вероятность ошибки.

**Альтернативная (более частая) формула:**
$$
\begin{aligned}
G &= \sum_{k=1}^{K} p_k (1 - p_k) \\
&= \sum_{k=1}^{K} (p_k - p_k^2) \\
&= \sum_{k=1}^{K} p_k - \sum_{k=1}^{K} p_k^2 \\
\end{aligned}
$$
Так как $\sum_{k=1}^{K} p_k = 1$ (сумма долей всех классов равна 1), получаем:
$$
\boxed{
G = 1 - \sum_{k=1}^{K} p_k^2
}
$$

* **Объяснение:** Эта форма эквивалентна предыдущей. Она вычитает из 1 сумму квадратов долей классов. Чем "чище" узел (доминирует один класс), тем больше один из $p_k$ близок к 1, а остальные к 0, и тем больше $\sum p_k^2$ близка к 1, а $G$ близка к 0.
* **Пример:**
  * Узел с классами {0: 10, 1: 0} (чистый): $p_0=1, p_1=0$. $G = 1 - (1^2 + 0^2) = 0$.
  * Узел с классами {0: 5, 1: 5} (макс. нечистота для K=2): $p_0=0.5, p_1=0.5$. $G = 1 - (0.5^2 + 0.5^2) = 1 - (0.25 + 0.25) = 0.5$.

#### 1.1.2 Entropy (Энтропия Шеннона)

**Определение:** Мера неопределенности или "хаоса" в распределении классов в узле, основанная на теории информации. Измеряет, сколько в среднем "бит" информации нужно для кодирования класса случайно выбранного объекта из узла.

**Формула:**
$$
\boxed{
H = - \sum_{k=1}^{K} p_k \log_2(p_k)
}
$$

* **$p_k$**: Доля объектов класса $k$ в узле.
* **$\log_2(p_k)$**: Логарифм по основанию 2. Часто используются и другие основания (натуральный log), это меняет масштаб энтропии, но не положение максимума/минимума, поэтому на выбор лучшего сплита не влияет.
* **Соглашение:** Если $p_k = 0$, то считаем $p_k \log_2(p_k) = 0$.
* **Объяснение:** Член $-p_k \log_2(p_k)$ представляет "информационное содержание" наблюдения класса $k$. Сумма по всем классам дает ожидаемое информационное содержание (энтропию). Энтропия равна 0 для чистого узла ($p_k=1$ для одного $k$) и максимальна при равномерном распределении классов ($p_k = 1/K$ для всех $k$).
* **Пример:**
  * Узел {0: 10, 1: 0}: $p_0=1, p_1=0$. $H = - [1 \log_2(1) + 0 \cdot \log_2(0)] = - [1 \cdot 0 + 0] = 0$.
  * Узел {0: 5, 1: 5}: $p_0=0.5, p_1=0.5$. $H = - [0.5 \log_2(0.5) + 0.5 \log_2(0.5)] = - [0.5 \cdot (-1) + 0.5 \cdot (-1)] = - [-0.5 - 0.5] = 1$.

### 1.2 Критерий "Чистоты" Узла - Регрессия

#### 1.2.1 Variance Reduction (Уменьшение Дисперсии)

**Определение:** В задачах регрессии целью является минимизация разброса (дисперсии) значений целевой переменной $y$ внутри листьев. Критерием чистоты узла служит сама дисперсия $y$ в этом узле.

**Формула Дисперсии в Узле:**
$$
\boxed{
Var_{node} = \frac{1}{N_{node}} \sum_{i \in \text{node}} (y^{(i)} - \bar{y}_{node})^2
}
$$

* **$N_{node}$**: Число объектов в текущем узле.
* **$y^{(i)}$**: Истинное значение целевой переменной для $i$-го объекта в узле.
* **$\bar{y}_{node}$**: Среднее значение $y$ по всем объектам в узле ($\bar{y}_{node} = \frac{1}{N_{node}} \sum_{i \in \text{node}} y^{(i)}$). Это значение обычно и является предсказанием дерева в данном узле, если он станет листом.
* **Объяснение:** Формула измеряет средний квадрат отклонения значений $y$ от их среднего в узле. Это то же самое, что **MSE (Среднеквадратичная ошибка)**, если бы мы предсказывали среднее значение для всех объектов в узле. Цель сплита - найти такое разделение, чтобы суммарная (взвешенная) дисперсия в дочерних узлах была как можно меньше.

### 1.3 Критерий Качества Сплита: Information Gain (IG)

**Определение:** Насколько уменьшилась нечистота (Gini, Entropy) или дисперсия (Variance) после выполнения сплита по сравнению с родительским узлом. Мы выбираем сплит, который максимизирует этот прирост.

**Формула:**
$$
\boxed{
IG(\text{Parent, Split}) = \text{Impurity}(\text{Parent}) - \sum_{c \in \text{Children}} \frac{N_c}{N_{\text{Parent}}} \text{Impurity}(\text{Child } c)
}
$$

* **$\text{Impurity}(\text{Parent})$**: Значение критерия нечистоты (Gini, Entropy или Variance) в родительском узле (до сплита).
* **$\text{Children}$**: Множество дочерних узлов, полученных в результате сплита (обычно два: левый и правый).
* **$\text{Impurity}(\text{Child } c)$**: Значение критерия нечистоты в дочернем узле $c$.
* **$N_c$**: Число объектов, попавших в дочерний узел $c$.
* **$N_{\text{Parent}}$**: Общее число объектов в родительском узле ($N_{\text{Parent}} = \sum N_c$).
* **$\frac{N_c}{N_{\text{Parent}}}$**: Доля объектов, ушедших в дочерний узел $c$.
* **$\sum_{c \in \text{Children}} \frac{N_c}{N_{\text{Parent}}} \text{Impurity}(\text{Child } c)$**: Средневзвешенное значение нечистоты в дочерних узлах.
* **Объяснение:** Мы вычитаем из исходной нечистоты родителя средневзвешенную нечистоту детей. Чем больше это значение (IG), тем "чище" стали дочерние узлы в среднем, и тем лучше данный сплит.

**Пример Расчета IG (для Gini):**

* Родитель: {Class 0: 10, Class 1: 10}, $N_p=20$. $G_{parent} = 1 - (0.5^2 + 0.5^2) = 0.5$.
* Сплит 1:
  * Левый ребенок (Child L): {Class 0: 8, Class 1: 2}, $N_L=10$. $p_0=0.8, p_1=0.2$. $G_L = 1 - (0.8^2 + 0.2^2) = 1 - (0.64 + 0.04) = 0.32$.
  * Правый ребенок (Child R): {Class 0: 2, Class 1: 8}, $N_R=10$. $p_0=0.2, p_1=0.8$. $G_R = 1 - (0.2^2 + 0.8^2) = 1 - (0.04 + 0.64) = 0.32$.
  * $IG_1 = G_{parent} - (\frac{N_L}{N_p} G_L + \frac{N_R}{N_p} G_R) = 0.5 - (\frac{10}{20} \cdot 0.32 + \frac{10}{20} \cdot 0.32) = 0.5 - (0.5 \cdot 0.32 + 0.5 \cdot 0.32) = 0.5 - 0.32 = 0.18$.
* Сплит 2 (Идеальный):
  * Левый ребенок (Child L): {Class 0: 10, Class 1: 0}, $N_L=10$. $G_L = 0$.
  * Правый ребенок (Child R): {Class 0: 0, Class 1: 10}, $N_R=10$. $G_R = 0$.
  * $IG_2 = G_{parent} - (\frac{10}{20} \cdot 0 + \frac{10}{20} \cdot 0) = 0.5 - 0 = 0.5$.
* Вывод: Сплит 2 лучше, так как $IG_2 > IG_1$.

### 1.4 Cost Complexity Pruning (CCP)

**Определение:** Метод обрезки дерева после его построения для борьбы с переобучением путем минимизации функционала, штрафующего за сложность дерева (число листьев).

**Функционал Стоимости-Сложности:**
$$
\boxed{
R_\alpha(T) = R(T) + \alpha |T|
}
$$

* **$T$**: Рассматриваемое дерево или поддерево.
* **$R(T)$**: Суммарная ошибка или нечистота листьев дерева $T$. Рассчитывается как сумма нечистот (Gini, Entropy, MSE) по всем листьям дерева: $R(T) = \sum_{l \in \text{Leaves}(T)} N_l \cdot \text{Impurity}(l)$, где $N_l$ - число объектов в листе $l$.
* **$|T|$**: Количество листьев в дереве $T$. Мера сложности дерева.
* **$\alpha \ge 0$**: Параметр сложности (гиперпараметр). Контролирует баланс между точностью ($R(T)$) и сложностью ($|T|$).
  * При $\alpha=0$, $R_0(T) = R(T)$, выбирается самое большое (необрезанное) дерево.
  * При увеличении $\alpha$, штраф за сложность растет, и алгоритм предпочитает обрезать больше ветвей, выбирая меньшие поддеревья.
* **Объяснение:** Алгоритм ищет такое поддерево $T$, которое минимизирует $R_\alpha(T)$ для заданного $\alpha$. Варьируя $\alpha$, можно получить последовательность вложенных оптимально обрезанных деревьев. Оптимальное значение `ccp_alpha` для использования в модели подбирается по кросс-валидации.

## Часть 2: Ансамблевые Методы

### 2.1 Bagging (Bootstrap Aggregating)

**Определение:** Метод ансамблирования, где $B$ базовых моделей обучаются на $B$ бутстрэп-выборках (выборки с возвращением из исходных данных), а их предсказания агрегируются.

**Агрегация Предсказаний:**
Пусть $h_b(x)$ - предсказание $b$-й базовой модели (обученной на $b$-й бутстрэп-выборке) для объекта $x$.
Итоговое предсказание ансамбля $H(x)$:

* **Регрессия:** Среднее арифметическое.
    $$
    \boxed{
    H(x) = \frac{1}{B} \sum_{b=1}^{B} h_b(x)
    }
    $$
* **Классификация (Голосование большинством):**
    $$
    \boxed{
    H(x) = \text{argmax}_{k \in \{1, ..., K\}} \sum_{b=1}^{B} \mathbb{I}(h_b(x) = k)
    }
    $$
  * $\mathbb{I}(\cdot)$ - индикаторная функция (1, если условие верно, 0 иначе). Мы выбираем класс $k$, который был предсказан наибольшим числом базовых моделей.
* **Классификация (Усреднение вероятностей):** Если базовые модели $h_b(x)$ возвращают вектор вероятностей классов $[p_{b1}(x), ..., p_{bK}(x)]$, то:
    $$
    \text{Вероятности ансамбля: } \hat{p}_k(x) = \frac{1}{B} \sum_{b=1}^{B} p_{bk}(x)
    $$
    $$
    \boxed{
    H(x) = \text{argmax}_{k \in \{1, ..., K\}} \hat{p}_k(x)
    }
    $$
  * **Объяснение:** Усреднение вероятностей часто дает более гладкие и надежные результаты, чем жесткое голосование.

### 2.2 Random Forest (RF)

RF использует Bagging для деревьев решений с добавлением Random Subspace. Формулы агрегации предсказаний те же, что и для Bagging (см. выше). Основные формулы, специфичные для RF, касаются оценки важности признаков.

#### 2.2.1 Mean Decrease in Impurity (MDI) / Gini Importance

**Логика Расчета (Не единая формула, а алгоритм):**

1. Для каждого дерева $T_b$ в ансамбле:
    * Для каждого внутреннего узла $v$ в дереве $T_b$:
        * Если для сплита в узле $v$ использовался признак $j$:
            * Рассчитать уменьшение нечистоты (Impurity Decrease), достигнутое этим сплитом: $\Delta I(v) = N_v \cdot \text{Impurity}(v) - (N_{v,L} \cdot \text{Impurity}(v_L) + N_{v,R} \cdot \text{Impurity}(v_R))$. (Где $N$ - число объектов в узлах).
            * Добавить $\Delta I(v)$ к суммарному вкладу признака $j$ для дерева $T_b$.
2. Усреднить суммарный вклад каждого признака $j$ по всем деревьям $B$.
3. Нормализовать полученные важности (например, чтобы их сумма была равна 1).

* **Объяснение:** MDI оценивает, насколько в среднем каждый признак помогал уменьшить нечистоту узлов при построении деревьев на *обучающей* выборке.

#### 2.2.2 Permutation Importance / Mean Decrease in Accuracy (MDA)

**Логика Расчета (Алгоритм):**

1. Обучить ансамбль RF.
2. Рассчитать базовую метрику качества $S_{base}$ на OOB-выборке (или валидационной/тестовой).
3. Для каждого признака $j$ (от 1 до $n$):
    a.  Сохранить копию OOB-выборки (или val/test).
    b.  Перемешать значения **только** $j$-го столбца (признака $j$) в этой копии.
    c.  Сделать предсказания с помощью **обученного** RF на данных с перемешанным $j$-м признаком.
    d.  Рассчитать метрику качества $S_j$ на этих предсказаниях.
    e.  Рассчитать важность признака $j$: $Imp_j = S_{base} - S_j$.
4. (Опционально) Повторить шаги 3.b-3.e несколько раз и усреднить $Imp_j$ для стабильности.
5. (Опционально) Нормализовать важности $Imp_j$.

* **Объяснение:** MDA оценивает, насколько падает качество модели на "новых" данных, если разорвать связь между признаком $j$ и целевой переменной путем его перемешивания. Чем больше падение, тем важнее признак.

### 2.3 Gradient Boosting Machines (GBM)

#### 2.3.1 Обновление Предсказания Ансамбля

**Определение:** На каждой итерации $m$ предсказание ансамбля $F_m(x)$ обновляется добавлением предсказания новой базовой модели $h_m(x)$ (обученной на псевдо-остатках), умноженного на скорость обучения $\nu$.

**Формула:**
$$
\boxed{
F_m(x) = F_{m-1}(x) + \nu \cdot h_m(x)
}
$$

* **$F_m(x)$**: Предсказание ансамбля после добавления $m$-й модели.
* **$F_{m-1}(x)$**: Предсказание ансамбля на предыдущем шаге.
* **$\nu$**: Скорость обучения (learning rate, eta). Малое положительное число (0 < $\nu$ <= 1).
* **$h_m(x)$**: Предсказание $m$-й базовой модели (дерева).
* **Объяснение:** Мы делаем небольшой шаг ($\nu \cdot h_m(x)$) в направлении, которое предсказывает $m$-я модель (направлении анти-градиента), чтобы скорректировать предсказание ансамбля.

#### 2.3.2 Вычисление Псевдо-Остатков (Targets для $h_m$)

**Определение:** Целевая переменная для обучения $m$-й базовой модели $h_m(x)$. Она равна отрицательному градиенту функции потерь $L$, вычисленному по предсказанию ансамбля на *предыдущем* шаге $F_{m-1}$.

**Общая Формула:**
$$
\boxed{
r_{im} = - \left[ \frac{\partial L(y_i, F(x_i))}{\partial F(x_i)} \right]_{F(x_i) = F_{m-1}(x_i)}
}
$$

* **$r_{im}$**: Псевдо-остаток для $i$-го объекта на $m$-й итерации.
* **$L(y_i, F(x_i))$**: Дифференцируемая функция потерь для $i$-го объекта.
* **$\frac{\partial L}{\partial F}$**: Частная производная функции потерь по ее второму аргументу (предсказанию модели $F$).
* **$[\dots]_{F=F_{m-1}}$**: Означает, что производная вычисляется в точке, где предсказание равно предсказанию ансамбля на предыдущем шаге $F_{m-1}(x_i)$.

**Примеры для Конкретных Функций Потерь:**

* **MSE (для Регрессии):**
  * $L(y_i, F) = \frac{1}{2}(y_i - F)^2$
  * $\frac{\partial L}{\partial F} = \frac{1}{2} \cdot 2 (y_i - F) \cdot (-1) = -(y_i - F) = F - y_i$
  * $r_{im} = - [F_{m-1}(x_i) - y_i] = y_i - F_{m-1}(x_i)$
  * **Объяснение:** Для MSE псевдо-остаток - это просто **обычный остаток** (разница между истинным значением и предсказанием предыдущего ансамбля).

* **Log Loss (для Бинарной Классификации):**
  * $F$ здесь - это **логит** (выход до сигмоиды), $p = \sigma(F)$.
  * $L(y_i, p) = -[y_i \log(p) + (1-y_i)\log(1-p)]$
  * Нужна производная по $F$: $\frac{\partial L}{\partial F} = \frac{\partial L}{\partial p} \cdot \frac{\partial p}{\partial F}$.
  * Мы выводили ранее: $\frac{\partial L}{\partial p} = -\frac{y_i - p}{p(1-p)}$ и $\frac{\partial p}{\partial F} = \sigma'(F) = p(1-p)$.
  * $\frac{\partial L}{\partial F} = -\frac{y_i - p}{p(1-p)} \cdot p(1-p) = -(y_i - p) = p - y_i$
  * $r_{im} = - [p_{m-1}(x_i) - y_i] = y_i - p_{m-1}(x_i)$
    * Где $p_{m-1}(x_i) = \sigma(F_{m-1}(x_i))$ - предсказанная **вероятность** на предыдущем шаге.
  * **Объяснение:** Для Log Loss псевдо-остаток - это **разница между истинной меткой (0 или 1) и предсказанной вероятностью** предыдущего ансамбля. Новое дерево $h_m(x)$ будет учиться предсказывать это расхождение, чтобы скорректировать логит $F(x)$.
