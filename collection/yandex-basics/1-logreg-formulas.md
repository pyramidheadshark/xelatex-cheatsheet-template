# Детальный Вывод Формул: Логистическая Регрессия

*(Используем обозначения: $m$ - число объектов, $n$ - число признаков (не считая фиктивного), $x^{(i)}$ - вектор признаков $i$-го объекта ($[x_0^{(i)}, x_1^{(i)}, ..., x_n^{(i)}]^T$, где $x_0^{(i)}=1$), $y^{(i)}$ - истинный класс $i$-го объекта (0 или 1), $w$ - вектор весов ($[w_0, w_1, ..., w_n]^T$), $\hat{p}^{(i)}$ - предсказанная вероятность класса 1 для $i$-го объекта, $J(w)$ - функция потерь).*

## 1. Сигмоидная Функция и ее Производная

Сигмоидная (логистическая) функция используется для преобразования линейного выхода модели (логита) в вероятность.

**Определение Сигмоиды:**
$$
\sigma(z) = \frac{1}{1 + e^{-z}}
$$

* Где $z$ - входное значение (логит).

**Производная Сигмоиды $\sigma'(z) = \frac{d\sigma}{dz}$:**
Для вывода используем правило дифференцирования частного $d/dz (u/v) = (u'v - uv')/v^2$ или запишем как $(1 + e^{-z})^{-1}$.
Используем цепное правило $d/dz (f(g(z))) = f'(g(z)) \cdot g'(z)$.
Пусть $f(u) = u^{-1}$, $g(z) = 1 + e^{-z}$. Тогда $f'(u) = -u^{-2}$, $g'(z) = \frac{d}{dz}(1 + e^{-z}) = -e^{-z}$.

$$
\begin{aligned}
\sigma'(z) &= \frac{d}{dz} (1 + e^{-z})^{-1} \\
&= -1 \cdot (1 + e^{-z})^{-2} \cdot (-e^{-z}) \\
&= \frac{e^{-z}}{(1 + e^{-z})^2} \\
&= \frac{1}{1 + e^{-z}} \cdot \frac{e^{-z}}{1 + e^{-z}} \\
\end{aligned}
$$
Теперь трюк: добавим и вычтем 1 в числителе второй дроби:
$$
\frac{e^{-z}}{1 + e^{-z}} = \frac{1 + e^{-z} - 1}{1 + e^{-z}} = \frac{1 + e^{-z}}{1 + e^{-z}} - \frac{1}{1 + e^{-z}} = 1 - \sigma(z)
$$
Подставляем обратно:
$$
\boxed{\sigma'(z) = \sigma(z) (1 - \sigma(z))}
$$

* **Объяснение:** Производная сигмоиды удобно выражается через саму функцию сигмоиды. Это свойство будет критически важно при вычислении градиента функции потерь.

## 2. Функция Потерь Log Loss (Вывод из Максимального Правдоподобия)

Мы хотим найти такие веса $w$, которые максимизируют вероятность наблюдать наши данные $(\mathbf{X}, \mathbf{y})$.

**Шаг 1: Вероятность одного наблюдения $y^{(i)}$**
Предполагаем, что $y^{(i)}$ следует распределению Бернулли с параметром $p = \hat{p}^{(i)} = \sigma(w^T x^{(i)})$. Функция вероятности для одного наблюдения $i$:
$$
P(y^{(i)} | x^{(i)}; w) = (\hat{p}^{(i)})^{y^{(i)}} (1 - \hat{p}^{(i)})^{1 - y^{(i)}}
$$

* **Объяснение:** Если $y^{(i)}=1$, то $P = (\hat{p}^{(i)})^1 (1-\hat{p}^{(i)})^0 = \hat{p}^{(i)}$. Если $y^{(i)}=0$, то $P = (\hat{p}^{(i)})^0 (1-\hat{p}^{(i)})^1 = 1-\hat{p}^{(i)}$. Эта формула компактно записывает оба случая.

**Шаг 2: Функция Правдоподобия (Likelihood) для всей выборки**
Предполагая независимость наблюдений, правдоподобие всей выборки равно произведению вероятностей отдельных наблюдений:
$$
L(w) = P(\mathbf{y} | \mathbf{X}; w) = \prod_{i=1}^{m} P(y^{(i)} | x^{(i)}; w) = \prod_{i=1}^{m} (\hat{p}^{(i)})^{y^{(i)}} (1 - \hat{p}^{(i)})^{1 - y^{(i)}}
$$

* **Объяснение:** Мы ищем $w$, максимизирующие эту функцию - то есть делающие наблюдаемые данные наиболее "вероятными".

**Шаг 3: Логарифмическая Функция Правдоподобия (Log-Likelihood)**
Максимизировать $L(w)$ эквивалентно максимизации ее логарифма $\log L(w)$ (т.к. $\log$ - монотонная функция). Логарифмирование превращает произведение в сумму и упрощает дифференцирование.
$$
\begin{aligned}
\log L(w) &= \log \left( \prod_{i=1}^{m} (\hat{p}^{(i)})^{y^{(i)}} (1 - \hat{p}^{(i)})^{1 - y^{(i)}} \right) \\
&= \sum_{i=1}^{m} \log \left( (\hat{p}^{(i)})^{y^{(i)}} (1 - \hat{p}^{(i)})^{1 - y^{(i)}} \right) \\
&= \sum_{i=1}^{m} [ \log((\hat{p}^{(i)})^{y^{(i)}}) + \log((1 - \hat{p}^{(i)})^{1 - y^{(i)}}) ] \\
&= \sum_{i=1}^{m} [ y^{(i)} \log(\hat{p}^{(i)}) + (1 - y^{(i)}) \log(1 - \hat{p}^{(i)}) ]
\end{aligned}
$$

* **Объяснение:** Мы применили свойства логарифмов: $\log(ab) = \log(a) + \log(b)$ и $\log(a^k) = k \log(a)$.

**Шаг 4: Переход к Функции Потерь (Минимизация)**
Максимизация $\log L(w)$ эквивалентна минимизации отрицательного логарифма правдоподобия $-\log L(w)$. Для получения средней ошибки по выборке, делим на $m$:
$$
J(w) = -\frac{1}{m} \log L(w) = -\frac{1}{m} \sum_{i=1}^{m} [ y^{(i)} \log(\hat{p}^{(i)}) + (1 - y^{(i)}) \log(1 - \hat{p}^{(i)}) ]
$$
Подставляя $\hat{p}^{(i)} = \sigma(w^T x^{(i)})$:
$$
\boxed{
J(w) = -\frac{1}{m} \sum_{i=1}^{m} [ y^{(i)} \log(\sigma(w^T x^{(i)})) + (1 - y^{(i)}) \log(1 - \sigma(w^T x^{(i)})) ]
}
$$

* **Объяснение:** Это и есть функция потерь Log Loss (или Бинарная Кросс-Энтропия), которую мы минимизируем с помощью градиентного спуска. Она имеет вероятностное обоснование через ММП и является выпуклой.

## 3. Градиент Функции Потерь Log Loss

Нам нужно найти частную производную $J(w)$ по каждому весу $w_k$ (для $k=0, 1, ..., n$).

**Шаг 1: Дифференцируем $J(w)$ по $w_k$**
$$
\frac{\partial J}{\partial w_k} = \frac{\partial}{\partial w_k} \left( -\frac{1}{m} \sum_{i=1}^{m} [ y^{(i)} \log(\hat{p}^{(i)}) + (1 - y^{(i)}) \log(1 - \hat{p}^{(i)}) ] \right)
$$
Выносим константу и сумму:
$$
= -\frac{1}{m} \sum_{i=1}^{m} \frac{\partial}{\partial w_k} [ y^{(i)} \log(\hat{p}^{(i)}) + (1 - y^{(i)}) \log(1 - \hat{p}^{(i)}) ]
$$
Дифференцируем слагаемые внутри скобок по $w_k$. Используем цепное правило: $\frac{d}{dw_k} (\dots) = \frac{d}{d\hat{p}^{(i)}} (\dots) \cdot \frac{d\hat{p}^{(i)}}{dw_k}$.
$$
\frac{\partial}{\partial \hat{p}^{(i)}} [ y^{(i)} \log(\hat{p}^{(i)}) + (1 - y^{(i)}) \log(1 - \hat{p}^{(i)}) ] = y^{(i)} \frac{1}{\hat{p}^{(i)}} + (1 - y^{(i)}) \frac{1}{1 - \hat{p}^{(i)}} (-1)
$$
$$
= \frac{y^{(i)}}{\hat{p}^{(i)}} - \frac{1 - y^{(i)}}{1 - \hat{p}^{(i)}} = \frac{y^{(i)}(1 - \hat{p}^{(i)}) - (1 - y^{(i)})\hat{p}^{(i)}}{\hat{p}^{(i)}(1 - \hat{p}^{(i)})}
$$
$$
= \frac{y^{(i)} - y^{(i)}\hat{p}^{(i)} - \hat{p}^{(i)} + y^{(i)}\hat{p}^{(i)}}{\hat{p}^{(i)}(1 - \hat{p}^{(i)})} = \frac{y^{(i)} - \hat{p}^{(i)}}{\hat{p}^{(i)}(1 - \hat{p}^{(i)})}
$$

* **Объяснение:** Нашли производную внутреннего выражения по $\hat{p}^{(i)}$.

**Шаг 2: Находим $\frac{d\hat{p}^{(i)}}{dw_k}$**
Напомним, $\hat{p}^{(i)} = \sigma(z^{(i)})$ и $z^{(i)} = w^T x^{(i)} = \sum_{j=0}^{n} w_j x_j^{(i)}$.
Снова используем цепное правило: $\frac{d\hat{p}^{(i)}}{dw_k} = \frac{d\sigma(z^{(i)})}{dz^{(i)}} \cdot \frac{dz^{(i)}}{dw_k}$.
Мы знаем производную сигмоиды: $\frac{d\sigma(z^{(i)})}{dz^{(i)}} = \sigma(z^{(i)})(1 - \sigma(z^{(i)})) = \hat{p}^{(i)}(1 - \hat{p}^{(i)})$.
Найдем производную логита $z^{(i)}$ по $w_k$:
$$
\frac{dz^{(i)}}{dw_k} = \frac{d}{dw_k} \left( \sum_{j=0}^{n} w_j x_j^{(i)} \right) = x_k^{(i)}
$$

* **Объяснение:** Производная суммы по $w_k$ равна коэффициенту при $w_k$, то есть $x_k^{(i)}$.

Теперь собираем $\frac{d\hat{p}^{(i)}}{dw_k}$:
$$
\frac{d\hat{p}^{(i)}}{dw_k} = \hat{p}^{(i)}(1 - \hat{p}^{(i)}) x_k^{(i)}
$$

**Шаг 3: Собираем градиент $\frac{\partial J}{\partial w_k}$**
Подставляем производные, найденные в Шагах 1 и 2:
$$
\frac{\partial J}{\partial w_k} = -\frac{1}{m} \sum_{i=1}^{m} \left[ \left( \frac{y^{(i)} - \hat{p}^{(i)}}{\hat{p}^{(i)}(1 - \hat{p}^{(i)})} \right) \cdot \left( \hat{p}^{(i)}(1 - \hat{p}^{(i)}) x_k^{(i)} \right) \right]
$$
Множители $\hat{p}^{(i)}(1 - \hat{p}^{(i)})$ сокращаются:
$$
= -\frac{1}{m} \sum_{i=1}^{m} (y^{(i)} - \hat{p}^{(i)}) x_k^{(i)}
$$
Меняем знак, чтобы избавиться от минуса перед суммой:
$$
\boxed{
\frac{\partial J}{\partial w_k} = \frac{1}{m} \sum_{i=1}^{m} (\hat{p}^{(i)} - y^{(i)}) x_k^{(i)}
}
$$

* **Объяснение:** Получили элегантную формулу для градиента. Она показывает, что вклад $i$-го объекта в градиент по весу $w_k$ пропорционален ошибке предсказания $(\hat{p}^{(i)} - y^{(i)})$ и значению $k$-го признака $x_k^{(i)}$ этого объекта. Формула идентична градиенту MSE, но $\hat{p}^{(i)}$ считается через сигмоиду.

**Векторная форма Градиента:**
Собирая производные по всем $k$ от 0 до $n$ в вектор:
$$
\boxed{
\nabla J(w) = \frac{1}{m} \mathbf{X}^T (\sigma(\mathbf{X}w) - \mathbf{y})
}
$$

* **Объяснение:** $\sigma(\mathbf{X}w)$ - вектор предсказанных вероятностей $\hat{\mathbf{p}}$. $(\sigma(\mathbf{X}w) - \mathbf{y})$ - вектор ошибок $m \times 1$. Умножение на транспонированную матрицу признаков $\mathbf{X}^T$ ($(n+1) \times m$) суммирует ошибки с нужными весами (признаками) и дает итоговый вектор градиента $(n+1) \times 1$.

## 4. Обновление Весов в Градиентном Спуске

Используя вычисленный градиент, обновляем веса на каждой итерации:

**Скалярная форма (для каждого $k = 0, 1, ..., n$):**
$$
\boxed{
w_k := w_k - \alpha \frac{\partial J}{\partial w_k} = w_k - \alpha \left( \frac{1}{m} \sum_{i=1}^{m} (\hat{p}^{(i)} - y^{(i)}) x_k^{(i)} \right)
}
$$
*(Примечание: Для Mini-batch GD сумма берется по объектам $i$ в батче, и делится на размер батча $B$ вместо $m$.)*

**Векторная форма:**
$$
\boxed{
w := w - \alpha \nabla J(w) = w - \alpha \left( \frac{1}{m} \mathbf{X}^T (\sigma(\mathbf{X}w) - \mathbf{y}) \right)
}
$$

* **Объяснение:** Мы делаем шаг в направлении, противоположном градиенту, с шагом, контролируемым скоростью обучения $\alpha$. Этот процесс повторяется итеративно до сходимости.
