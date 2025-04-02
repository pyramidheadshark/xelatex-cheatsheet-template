# 📝 Шаблон Шпаргалок на XeLaTeX

[![LaTeX](https://img.shields.io/badge/LaTeX-XeLaTeX-blue.svg)](https://www.latex-project.org/)
[![TeX Live](https://img.shields.io/badge/TeX%20Live-2023%2B-brightgreen.svg)](https://www.tug.org/texlive/)
[![Minted](https://img.shields.io/badge/Syntax%20Highlighting-Minted-orange.svg)](https://github.com/gpoore/minted)
<!-- ИЗМЕНЕНИЕ: Добавлен значок PGFPlots -->
[![PGFPlots](https://img.shields.io/badge/Plotting-PGFPlots-purple.svg)](https://ctan.org/pkg/pgfplots)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

🚀 **Посмотреть Пример PDF (Шпаргалка по Pandas):**

*   📄 **[Скачать/Посмотреть Пример Шпаргалки (PDF)](docs/example.pdf)** *(Нажмите для просмотра или загрузки файла `docs/example.pdf` из этого репозитория)*

---

## 🖼️ Скриншоты Примера (Pandas Cheatsheet)


<p align="center">
  <img src="docs/screenshot1.png" width="80%">
</p>

---

## 📝 Обзор Проекта

Этот репозиторий содержит гибкий и настраиваемый **шаблон на XeLaTeX** (`cheatsheet-template.tex`), предназначенный для создания профессионально выглядящих шпаргалок, справочных материалов и конспектов. Шаблон использует возможности XeLaTeX для работы с современными шрифтами, `tcolorbox` для создания стильных блоков, `minted` для высококачественной подсветки синтаксиса кода, `pgfplots` для встраивания графиков и `biblatex` для управления библиографией.

В качестве **демонстрационного примера** включена подробная шпаргалка по библиотеке **Pandas** для анализа данных (`mains/main-pandas.tex`, `contents/cheatsheet-pandas.tex`), показывающая различные возможности шаблона в действии.

**Ключевые возможности шаблона:**

1.  Многоколоночная верстка (`multicol`).
2.  Поддержка Unicode и современных шрифтов (`fontspec`).
3.  Настраиваемые блоки для текста, кода, примеров, предупреждений (`tcolorbox`).
4.  Подсветка синтаксиса для множества языков программирования (`minted` + `Pygments`).
5.  <!-- ИЗМЕНЕНИЕ: Добавлена возможность PGFPlots -->
    **Встроенная генерация графиков** с использованием `pgfplots` для визуализации данных непосредственно в документе с единым стилем.
6.  Автоматическое оглавление.
7.  Поддержка библиографии и сносок (`biblatex` + `biber`).
8.  Легкая адаптация под различные темы.

## 📂 Структура Проекта

```
├── mains/                    # Главные .tex файлы для каждой шпаргалки
│   └── main-pandas.tex       # Пример главного файла (Pandas)
├── contents/                 # Файлы с контентом шпаргалок
│   └── cheatsheet-pandas.tex # Пример контента (Pandas)
├── img/                      # Папка для внешних изображений (если нужны)
│   └── .gitkeep
├── build/                    # Рекомендуемый каталог для выходных файлов компиляции
│   └── .gitkeep
├── cheatsheet-template.tex   # Основной шаблон - ИЗМЕНЯТЬ С ОСТОРОЖНОСТЬЮ
├── literature.bib            # Файл библиографии
├── docs/                     # Документация, скриншоты, примеры PDF
│   ├── example.pdf
│   └── screenshot1.png
├── collection/               # Папка для собранных/архивных PDF
│   ├── examples/
│   │   └── main-pandas.pdf
│   └── internship-basics/
│       └── main-pandas.pdf
├── _minted*                  # Кеш minted (добавить в .gitignore)
├── .vscode/                  # Настройки VS Code (опционально)
├── .gitignore                # Файл для Git
└── README.md                 # Этот файл
```

*   **`cheatsheet-template.tex`**: Содержит все определения стилей, команд и окружений, включая настройки `pgfplots`.
*   **`mains/main-*.tex`**: Главный файл документа. Задает геометрию, подключает шаблон, контент, устанавливает заголовок/автора.
*   **`contents/cheatsheet-*.tex`**: Файлы с основным содержанием шпаргалки (текст, код, графики `pgfplots`).
*   **`literature.bib`**: Файл `biblatex` для ссылок.
*   **`img/`**: Папка для *внешних* изображений (если `pgfplots` недостаточно).
*   **`build/`**: Рекомендуемый каталог для временных файлов и финального PDF.

## ⚙️ Требования

Для компиляции этого шаблона необходимы:

1.  **Дистрибутив LaTeX:** **TeX Live (2023 или новее)** рекомендуется. Включает `xelatex`, `biber`, `latexmk` и необходимые пакеты (`fontspec`, `tcolorbox`, `minted`, `biblatex`, `multicol`, `hyperref`, `xcolor`, `fontawesome`, `pgfplots` <!-- ИЗМЕНЕНИЕ: Добавлен pgfplots --> и др.).
2.  **Шрифты:** Необходимо установить в систему следующие шрифты:
    *   **PT Sans** (Regular, Bold, Italic, Bold Italic) - [Скачать с Google Fonts](https://fonts.google.com/specimen/PT+Sans)
    *   **Liberation Mono** (Regular) - [Скачать](https://github.com/liberationfonts/liberation-fonts) или часто есть в Linux.
    *   *FontAwesome* (обычно включен в TeX Live как пакет `fontawesome5`).
3.  **Python:** (>= 3.6) Требуется для работы пакета `minted`. Должен быть доступен в `PATH`.
4.  **Библиотека Pygments:** Python-библиотека для подсветки синтаксиса (`minted`). Установка: `pip install Pygments`.
5.  <!-- ИЗМЕНЕНИЕ: Уточнено про pgfplots -->
    **pgfplots:** Пакет LaTeX для графиков. Обычно включен в TeX Live. Дополнительных *внешних* зависимостей (как Python для `minted`) не требует.

## 🛠️ Компиляция

Компиляция документа требует использования `xelatex` (из-за `fontspec`), опции `-shell-escape` (из-за `minted`). `pgfplots` сам по себе не требует `-shell-escape`, но так как он используется вместе с `minted`, флаг все равно нужен. Если используется библиография, нужен запуск `biber`.

**Рекомендуемый способ (использование `latexmk`):**

`latexmk` автоматически выполнит все шаги. **Запускайте команду из корневой директории проекта.**

**Компиляция с выводом в папку `build/`:**

```bash
# Убедитесь, что папка build существует (mkdir build)
latexmk -xelatex -shell-escape -output-directory=build mains/main-pandas.tex
```

*   `-xelatex`: Использовать `xelatex`.
*   `-shell-escape`: Разрешить запуск внешних программ (`minted`).
*   `-output-directory=build`: Поместить все файлы в `build/`.
*   `mains/main-pandas.tex`: Путь к главному файлу.

Итоговый PDF будет в `build/main-pandas.pdf`.

**Компиляция в текущей директории:**

```bash
latexmk -xelatex -shell-escape mains/main-pandas.tex
```

**Очистка временных файлов (`latexmk`):**

```bash
# Очистка в папке build
latexmk -c -output-directory=build mains/main-pandas.tex
# Полная очистка (включая PDF) в папке build
latexmk -C -output-directory=build mains/main-pandas.tex

# Очистка в текущей папке
latexmk -c mains/main-pandas.tex
latexmk -C mains/main-pandas.tex
```

## 🚀 Как Использовать Шаблон для Своей Шпаргалки

1.  **Создайте файлы:**
    *   В `mains/` создайте `main-yourtopic.tex` (скопируйте `main-pandas.tex`).
    *   В `contents/` создайте `cheatsheet-yourtopic.tex` (скопируйте `cheatsheet-pandas.tex`).
2.  **Настройте главный файл (`mains/main-yourtopic.tex`):**
    *   Измените `\cheatsheettitle` и `\cheatsheetauthor`.
    *   Убедитесь, что пути `\input` корректны:
        ```latex
        \input{cheatsheet-template.tex} % Относительно корня
        \input{contents/cheatsheet-yourtopic.tex} % Относительно корня, обновите имя
        ```
3.  **Наполните контентом (`contents/cheatsheet-yourtopic.tex`):**
    *   Добавьте секции (`\section{...}`).
    *   Используйте окружения (`textbox`, `codebox`, `myblock`, `alerttextbox`, `myexampleblock` и т.д.).
    *   Добавляйте код в `codebox`, указывая язык.
    *   Добавляйте *внешние* изображения в `img/` и используйте `\mygraphics{img/your-image.png}`.
    *   <!-- НОВЫЙ ПУНКТ: Добавление графиков -->
        **Добавляйте встроенные графики (`pgfplots`):**
        *   Используйте окружения `\begin{tikzpicture}` и `\begin{axis}[<options>]`.
        *   Применяйте предопределенные стили для единообразия:
            *   `cheatsheet plot style`: для общих графиков.
            *   `roc curve style`: для ROC-кривых.
            *   `pr curve style`: для Precision-Recall кривых.
        *   Пример:
            ```latex
            \begin{myblock}{График Пример}
            \begin{center} % Рекомендуется центрировать
            \begin{tikzpicture}
                \begin{axis}[cheatsheet plot style, title={Мой график}]
                    \addplot coordinates {(0,0) (1,1) (2,4)};
                    \addlegendentry{Линия 1}
                \end{axis}
            \end{tikzpicture}
            \end{center}
            \end{myblock}
            ```
4.  **Обновите библиографию (`literature.bib`):** Добавьте источники, используйте `\footcite{ключ}`. Если не нужна, закомментируйте `\printbibliography`.
5.  **Скомпилируйте** ваш `mains/main-yourtopic.tex` командой `latexmk` из корня проекта.

## 💡 Возможные Улучшения и Настройка

*   **Изменение цветов:** `\definecolor` в `cheatsheet-template.tex`.
*   **Изменение шрифтов:** `\setmainfont`, `\setmonofont` в `cheatsheet-template.tex`.
*   **Настройка блоков `tcolorbox`:** Определения окружений (`\newenvironment`, `\newtcblisting`) в `cheatsheet-template.tex`.
*   <!-- ИЗМЕНЕНИЕ: Добавлена настройка PGFPlots -->
    **Настройка стилей графиков `pgfplots`:** Определения стилей (`\pgfplotsset`) находятся в `cheatsheet-template.tex`. Можно изменять существующие (`cheatsheet plot style`, `roc curve style`, `pr curve style`) или добавлять новые.
*   **Добавление новых типов блоков:** Создайте новые окружения на основе `tcolorbox`.
*   **Изменение макета:** Настройки колонок (`multicols`) и геометрии (`geometry`) в `main-*.tex`.

## 📄 Лицензия

Этот проект распространяется под лицензией MIT.