# Определение температуры сплава

## Задача

Чтобы оптимизировать производственные расходы, металлургический комбинат решил уменьшить потребление электроэнергии на этапе обработки стали. Для этого комбинату нужно контролировать температуру сплава. Наша задача — построить модель, которая будет её предсказывать.

Качество модели оценивается по метрике MAE. Пороговое значение - 6.8.

## Описание данных 

**Для выполнения задачи нам доступны 7 датасетов:**

-  **данные об электродах:**
   
   - `key` — номер партии;
   - `Начало нагрева дугой` — время начала нагрева;
   - `Конец нагрева дугой` — время окончания нагрева;
   - `Активная мощность` — значение активной мощности;
   - `Реактивная мощность` — значение реактивной мощности.
   
   
-  **данные о подаче сыпучих материалов (объём):**
   
   - `key` — номер партии;
   - `Bulk 1 … Bulk 15` — объём подаваемого материала.
   
   
-  **данные о подаче сыпучих материалов (время):**

   - `key` — номер партии;
   - `Bulk 1 … Bulk 15` — время подачи материала.


-  **данные о продувке сплава газом:**

   - `key` — номер партии;
   - `Газ 1` — объём подаваемого газа.
   
   
-  **результаты измерения температуры:**

   - `key` — номер партии;
   - `Время замера` — время замера;
   - `Температура` — значение температуры.
   
   
-  **данные о проволочных материалах (объём):**

   - `key` — номер партии;
   - `Wire 1 … Wire 15` — объём подаваемых проволочных материалов.
   
   
-  **данные о проволочных материалах (время):**

   - `key` — номер партии;
   - `Wire 1 … Wire 15` — время подачи проволочных материалов.
   
   
**Целевой признак**

- `Конечное значение температуры для каждой партии` 

## Описание проекта

Каждый датасет был проанализирован и предобработан (исследованы аномальные значения признаков, заполнены пропуски, изучены распределения признаков и выявлены ошибки в сборе данных). Затем мы агрегировали имеющиеся признаки по партиям и провели feature engineering, добавив 6 новых признаков.

На следующем этапе датасеты были объединены в один, а итоговый датафрейм был повторно проанализирован, отобраны полезные признаки (при наличии мультиколлинеарности).

Для обучения и проверки моделей использовались две выборки - обучающая и тестовая (75% и 25% данных соответственно). Для линейных моделей и регрессии опорных векторов данные были приведены к одному масштабу. Значения метрик моделей сравнивались на основе 5-фолдовой кросс-валидации.

Для тестирования были выбраны следующие модели:

- линейные модели: Lasso, Ridge и ElasticNet;
- регрессия опорных векторов(SVR);
- модель случайного леса;
- модели градиентного бустинга: XGBoost, CatBoost и LightGBM.


## Итоговый вывод

Все модели прошли проверку на адекватность константной моделью и преодолели пороговое значение метрики. **Ошибка лучшей модели на тестовых данных - CatBoost - составила 5.67°.** Модель данного фреймворка можно рекоммендовать для внедрения на производстве. Отметим, что регрессия опорных векторов на обучающих данных показала близкую метрику и может также рассматриваться в качестве прототипа для решения задачи.

