# Предсказание эффективности противовирусных соединений против вируса гриппа

**Автор:** А. Любошенко  
**Организация:** НИЯУ МИФИ  
**Тип работы:** Курсовая работа по машинному обучению

---

## Описание проекта

Данная курсовая работа посвящена применению методов машинного обучения для предсказания биологической активности химических соединений в отношении вируса гриппа. Работа выполнена на выборке из **1001 соединения**, описанных **210 молекулярными дескрипторами** (RDKit).

### Целевые переменные

| Переменная | Единицы | Описание | Направление |
|---|---|---|---|
| IC50 | mM | Ингибирующая концентрация -- подавляет вирус на 50% | меньше -- лучше |
| CC50 | mM | Цитотоксическая концентрация -- убивает 50% клеток | больше -- лучше |
| SI | - | Индекс селективности = CC50 / IC50 | больше -- лучше |

### Задачи

Для каждой целевой переменной сформулированы задачи двух типов:

**Классификация (4 задачи):**
- CC50 > median -- вещество безопаснее средней отметки (баланс 50/50)
- IC50 > median -- вещество менее ингибирующее (баланс 50/50)
- SI > median -- вещество селективнее медианы (баланс 50/50)
- SI > 8.0 -- стандартный биологический критерий (дисбаланс 35.7% / 64.3%)

**Регрессия (3 задачи):**
- Предсказание log1p(CC50)
- Предсказание log1p(IC50)
- Предсказание log1p(SI)

### Модели

| Тип задачи | Модели |
|---|---|
| Классификация | Logistic Regression, Random Forest Classifier, LightGBM Classifier, SVC, KNN Classifier |
| Регрессия | Ridge Regression, Lasso Regression, Random Forest Regressor, LightGBM Regressor, SVR |

### Метрики

- **Классификация:** ROC-AUC (основная), F1-score, Accuracy
- **Регрессия:** R^2 (в log-пространстве), RMSE и MAE (в исходном масштабе через expm1)

---

## Структура проекта

```
fludrugml-mephi-coursework-efficiency-prediction/
|
|-- data_of_course_paper.csv         -- датасет: 1001 соединение, 213 столбцов (IC50, CC50, SI + 210 дескрипторов)
|
|-- eda.ipynb                        -- разведочный анализ данных (EDA)
|
|-- classification_cc50_median.ipynb -- классификация: CC50 > median
|-- classification_ic50_median.ipynb -- классификация: IC50 > median
|-- classification_si_median.ipynb   -- классификация: SI > median
|-- classification_si_8.ipynb        -- классификация: SI > 8.0
|
|-- regression_cc50.ipynb            -- регрессия: предсказание log1p(CC50)
|-- regression_ic50.ipynb            -- регрессия: предсказание log1p(IC50)
|-- regression_si.ipynb              -- регрессия: предсказание log1p(SI)
|
|-- report.pdf                       -- пояснительная записка (курсовая работа)
|
|-- requirements.txt                 -- зависимости Python
|-- .python-version                  -- версия Python (3.12)
|-- .gitignore                       -- исключения Git
```

---

## Воспроизведение результатов

### Требования

- Python 3.12
- Git
- ~2 GB свободного места на диске (зависимости + данные)

### Шаг 1. Клонирование репозитория

```bash
git clone https://github.com/Liuboshenko/fludrugml-mephi-coursework-efficiency-prediction.git
cd fludrugml-mephi-coursework-efficiency-prediction
```

### Шаг 2. Создание виртуального окружения

**Linux / macOS:**
```bash
python3.12 -m venv venv3.12
source venv3.12/bin/activate
```

**Windows:**
```bash
python3.12 -m venv venv3.12
venv3.12\Scripts\activate
```

### Шаг 3. Установка зависимостей

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### Шаг 4. Запуск Jupyter

```bash
jupyter lab
```

или

```bash
jupyter notebook
```

### Шаг 5. Порядок запуска ноутбуков

Рекомендуемый порядок запуска:

```
1. eda.ipynb                        -- обязательно первым: разведочный анализ и план препроцессинга
2. classification_cc50_median.ipynb
3. classification_ic50_median.ipynb
4. classification_si_median.ipynb
5. classification_si_8.ipynb
6. regression_cc50.ipynb
7. regression_ic50.ipynb
8. regression_si.ipynb
```

Каждый ноутбук самодостаточен: загружает `data_of_course_paper.csv` и выполняет весь препроцессинг независимо.

---

## Основные зависимости

| Пакет | Версия | Назначение |
|---|---|---|
| scikit-learn | 1.8.0 | классические ML-модели |
| lightgbm | 4.6.0 | градиентный бустинг LightGBM |
| xgboost | 3.2.0 | градиентный бустинг XGBoost |
| catboost | 1.2.10 | градиентный бустинг CatBoost |
| pandas | 3.0.2 | обработка табличных данных |
| numpy | 2.4.4 | численные вычисления |
| matplotlib | 3.10.8 | визуализация |
| seaborn | 0.13.2 | статистическая визуализация |
| plotly | 6.7.0 | интерактивная визуализация |
| imbalanced-learn | 0.14.1 | работа с дисбалансом классов |
| jupyterlab | 4.5.6 | интерактивная среда разработки |

Полный список зависимостей -- в файле `requirements.txt`.

---

## Данные

Файл `data_of_course_paper.csv` содержит:
- **1001 строку** (химических соединений)
- **213 столбцов**: IC50, CC50, SI -- целевые переменные; 210 числовых молекулярных дескрипторов, рассчитанных с помощью RDKit
- Все дескрипторы числовые; пропуски и константные признаки обрабатываются в `eda.ipynb`

---

## Документация

Пояснительная записка к курсовой работе -- `report.pdf`.
