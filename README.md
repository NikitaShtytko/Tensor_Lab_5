Лабораторная работа №5
====
# Цель лабораторной работы
Обучить нейронную сеть с использованием техники обучения Fine Tuning

# 1. С использованием примера [2], техники обучения Transfer Learning [1], оптимальной политики изменения темпа обучения, аугментации данных с оптимальными настройками обучить нейронную сеть EfficientNet-B0 (предварительно обученную на базе изображений imagenet) для решения задачи классификации изображений Food-101:

## Применённые политики изменения темпа обучения и аугментации данных
 
 ```
tf.keras.experimental.CosineDecayRestarts(0.0001, 10000, 1.0, 2.0)
tf.keras.layers.experimental.preprocessing.RandomFlip('horizontal')
```

### Графики обучения для сети EfficientNet-B0

* Синий график - на валидационных данных
* Оранжевый график - на обучении

**График метрики точности:** 
<img src="./logs/logs-base/epoch_categorical_accuracy.svg">

**График функции потерь:**
<img src="./logs/logs-base/epoch_loss.svg">

### Вывод:
Наилучшее значение точности на валидационных данных достигается на 21 эпохе и значении 67,78%.


# 2. С использованием техники обучения Fine Tuning дополнительно обучить нейронную сеть EfficientNet-B0 предварительно обученную в пункте 1:

### Графики обучения для сети EfficientNet-B0

* Синий график - запуск Fine Tuning после 21 эпох обучения (на следующей же эпохе, после достижения максимальной точности)

* Голубой график - запуск Fine Tuning после 16 эпох обучения (до момента достижения максимальной точности)

**Графики метрики точности:** 
<img src="./logs/fine-tuning/epoch_categorical_accuracy.svg">

**Графики функции потерь:**
<img src="./logs/fine-tuning/epoch_loss.svg">

### Вывод:

Как видно из графиков, синий график выигрывает как в метрике точности так и показывает меньшее значение потерь. Наилучшее значение точности достигается синим графиком на 4 эпохе при значении метрики точности 68,57% и значении потерь 1.186. В дальнейшем сеть начинает переобучаться.
	
	
# Анализ результатов

В результате первого эксперимента максимальная точность достигается на 21 эпохе и значении 67,78%. В результате же 2 эксперимента, наилучшее значение оказалось в случае запуска Fine Tuning сразу же после 21 эпохи. В таком случае максимальная точность достигается через 4 эпохи при значении 68,57%. Случай запуска Fine Tuning после 16 эпох так же улучшил максимальное значение точности, на 5 эпохе значение метрики точности 68,45% (дальнейшее обучение обоих случаев приводит к переобучению сети и следовательно возрастанию потерь при классификации). Запуск Fine Tuning после 21 и 16 эпох улучшает значение максимальной точности на 0,79% и 0,67% соответственно. Нельзя не отметить, что запуск Fine Tuning после 21 эпохи не только имеет большее максимальное значение точности и значение потерь, но так же сходится на 1 эпоху быстрее.
Таким образом оптимальным вариантом применения техники Fine Tuning в нашем случае, является запуск сразу после достижения максимального значения точности базовой моделью (модель из эксперимента 1), что приводит к улучшению максимального значения точности на 0,79%.
