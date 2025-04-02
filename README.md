# 📝 Шаблон Шпаргалок на XeLaTeX

[![LaTeX](https://img.shields.io/badge/LaTeX-XeLaTeX-blue.svg)](https://www.latex-project.org/)
[![TeX Live](https://img.shields.io/badge/TeX%20Live-2023%2B-brightgreen.svg)](https://www.tug.org/texlive/)
[![Minted](https://img.shields.io/badge/Syntax%20Highlighting-Minted-orange.svg)](https://github.com/gpoore/minted)
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

Этот репозиторий содержит гибкий и настраиваемый **шаблон на XeLaTeX** (`cheatsheet-template.tex`), предназначенный для создания профессионально выглядящих шпаргалок, справочных материалов и конспектов. Шаблон использует возможности XeLaTeX для работы с современными шрифтами, `tcolorbox` для создания стильных блоков, `minted` для высококачественной подсветки синтаксиса кода и `biblatex` для управления библиографией.

В качестве **демонстрационного примера** включена подробная шпаргалка по библиотеке **Pandas** для анализа данных (`mains/main-pandas.tex`, `contents/cheatsheet-pandas.tex`), показывающая различные возможности шаблона в действии.

**Ключевые возможности шаблона:**

1.  Многоколоночная верстка (`multicol`).
2.  Поддержка Unicode и современных шрифтов (`fontspec`).
3.  Настраиваемые блоки для текста, кода, примеров, предупреждений (`tcolorbox`).
4.  Подсветка синтаксиса для множества языков программирования (`minted` + `Pygments`).
5.  Автоматическое оглавление.
6.  Поддержка библиографии и сносок (`biblatex` + `biber`).
7.  Легкая адаптация под различные темы.

## 📂 Структура Проекта

```
├── mains/                    # Главные .tex файлы для каждой шпаргалки
│   └── main-pandas.tex       # Пример главного файла (Pandas)
├── contents/                 # Файлы с контентом шпаргалок
│   └── cheatsheet-pandas.tex # Пример контента (Pandas)
├── img/                      # Папка для изображений
│   └── pandas_plot_example.png # Пример изображения
├── build/                    # Рекомендуемый каталог для выходных файлов компиляции
├── cheatsheet-template.tex   # Основной шаблон - ИЗМЕНЯТЬ С ОСТОРОЖНОСТЬЮ
├── literature.bib            # Файл библиографии
└── README.md                 # Этот файл
```

*   **`cheatsheet-template.tex`**: Содержит все определения стилей, команд и окружений.
*   **`mains/main-*.tex`**: Главный файл документа. Задает геометрию страницы, подключает шаблон, контент, устанавливает заголовок/автора и запускает генерацию (`\documentclass`, `\input{../cheatsheet-template.tex}`, `\input{../contents/...}`). *Примечание: Пути к шаблону и контенту указываются относительно корня проекта при компиляции из корня.*
*   **`contents/cheatsheet-*.tex`**: Файлы с основным содержанием шпаргалки (текст, код, команды `\section`, `\begin{textbox}` и т.д.).
*   **`literature.bib`**: Файл `biblatex` для ссылок и библиографии.
*   **`img/`**: Папка для изображений, используемых в шпаргалке.
*   **`build/`**: Рекомендуемый каталог для временных файлов LaTeX и финального PDF (создается при компиляции с `-output-directory=build`).

## ⚙️ Требования

Для компиляции этого шаблона необходимы:

1.  **Дистрибутив LaTeX:** **TeX Live (2023 или новее)** рекомендуется. Включает `xelatex`, `biber`, `latexmk` и необходимые пакеты (`fontspec`, `tcolorbox`, `minted`, `biblatex`, `multicol`, `hyperref`, `xcolor`, `fontawesome` и др.).
2.  **Шрифты:** Необходимо установить в систему следующие шрифты:
    *   **PT Sans** (включая Regular, Bold, Italic, Bold Italic) - основной шрифт. [Скачать с Google Fonts](https://fonts.google.com/specimen/PT+Sans)
    *   **Liberation Mono** (включая Regular) - для листингов кода. Обычно поставляется с Linux или [можно скачать](https://github.com/liberationfonts/liberation-fonts).
    *   *FontAwesome* (обычно включен в TeX Live как пакет `fontawesome5`).
3.  **Python:** (Обычно >= 3.6) Требуется для работы пакета `minted`. Должен быть доступен в системном `PATH`.
4.  **Библиотека Pygments:** Python-библиотека для подсветки синтаксиса, используется `minted`. Устанавливается через pip:
    ```
    pip install Pygments
    # или
    pip3 install Pygments
    ```

## 🛠️ Компиляция

Компиляция документа требует использования `xelatex` (из-за `fontspec`), опции `-shell-escape` (из-за `minted`) и, если используется библиография, запуска `biber`.

**Рекомендуемый способ (использование `latexmk`):**

`latexmk` автоматически выполнит все необходимые шаги (`xelatex`, `biber`) нужное количество раз. **Запускайте команду из корневой директории проекта.**

**Чтобы скомпилировать и поместить все выходные файлы в папку `build/`:**

```
# Убедитесь, что папка build существует (mkdir build)
latexmk -xelatex -shell-escape -output-directory=build mains/main-pandas.tex
```

*   `latexmk`: Запускает автоматизатор.
*   `-xelatex`: Указывает использовать `xelatex`.
*   `-shell-escape`: Разрешает `xelatex` запускать внешние программы (нужно для `minted`).
*   `-output-directory=build`: Направляет все временные файлы (`.aux`, `.log`, кеш `minted` и т.д.) и итоговый PDF в папку `build`.
*   `mains/main-pandas.tex`: Путь к главному файлу для компиляции *относительно корня проекта*.

Итоговый PDF будет находиться в `build/main-pandas.pdf`.

**Чтобы скомпилировать в текущей директории (все файлы будут в корне):**

```
latexmk -xelatex -shell-escape mains/main-pandas.tex
```

**Очистка временных файлов (используя `latexmk`):**

```
# Очистка в папке build
latexmk -c -output-directory=build mains/main-pandas.tex
# Полная очистка (включая PDF) в папке build
latexmk -C -output-directory=build mains/main-pandas.tex

# Очистка в текущей папке (если компилировали без -output-directory)
latexmk -c mains/main-pandas.tex
latexmk -C mains/main-pandas.tex
```

## 🚀 Как Использовать Шаблон для Своей Шпаргалки

1.  **Создайте файлы:**
    *   В папке `mains/` создайте новый главный файл (например, `mains/main-yourtopic.tex`). Можно скопировать существующий `mains/main-pandas.tex`.
    *   В папке `contents/` создайте новый файл контента (например, `contents/cheatsheet-yourtopic.tex`). Можно скопировать существующий `contents/cheatsheet-pandas.tex`.
2.  **Настройте главный файл (`mains/main-yourtopic.tex`):**
    *   Измените `\cheatsheettitle` и `\cheatsheetauthor`.
    *   **Важно:** Убедитесь, что пути в командах `\input` корректны *относительно места запуска компиляции* (корня проекта). Они должны оставаться такими же, как в `main-pandas.tex`, если вы не меняли структуру глубже:
        ```
        \input{cheatsheet-template.tex}
        \input{contents/cheatsheet-yourtopic.tex} % <-- Измените только имя файла контента
        ```
3.  **Наполните контентом (`contents/cheatsheet-yourtopic.tex`):**
    *   Удалите или закомментируйте содержимое примера.
    *   Добавьте свои секции (`\section{...}`).
    *   Используйте окружения из `cheatsheet-template.tex` (`textbox`, `codebox`, `myblock`, `alerttextbox`, `myexampleblock` и т.д.) для форматирования вашего материала.
    *   Добавляйте код в блоки `codebox`, указывая язык программирования.
    *   Добавляйте изображения в папку `img/` и используйте `\mygraphics{img/your-image.png}`.
4.  **Обновите библиографию (`literature.bib`):**
    *   Добавьте свои источники в формате BibTeX.
    *   Используйте `\footcite{ключ}` или `\autocite{ключ}` в тексте для ссылок. Если библиография не нужна, удалите или закомментируйте `\printbibliography` из вашего `main-*.tex` файла.
5.  **Скомпилируйте** ваш `mains/main-yourtopic.tex`, используя команду `latexmk` из корневой директории проекта, как описано в разделе "Компиляция".

## 💡 Возможные Улучшения и Настройка

*   **Изменение цветов:** Определения цветов (`\definecolor`) находятся в `cheatsheet-template.tex`.
*   **Изменение шрифтов:** Команды `\setmainfont`, `\setmonofont` находятся в `cheatsheet-template.tex`. Убедитесь, что выбранные шрифты установлены в системе.
*   **Настройка блоков `tcolorbox`:** Параметры блоков (рамки, фон, отступы) можно изменить в определениях окружений (`\newenvironment`, `\newtcblisting`) в `cheatsheet-template.tex`.
*   **Добавление новых типов блоков:** Можно создать новые окружения на основе `tcolorbox` по аналогии с существующими.
*   **Изменение макета:** Настройки колонок (`multicols`) и геометрии страницы (`geometry`) находятся в `main-*.tex` файлах.

## 📄 Лицензия

Этот проект распространяется под лицензией MIT.