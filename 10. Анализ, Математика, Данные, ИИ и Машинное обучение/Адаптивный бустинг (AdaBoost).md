Алгоритм обучает первую базовую модель(допустим деревья решений) на тренировочном наборе.

Относительный вес некорректно предсказанных значений увеличивается.

Веса хорошо классифицированных объектов уменьшаются относительно весов неправильно классифицированных объектов.

На вход второй базовой модели подаются обновлённые веса и модель обучается, после чего вырабатываются прогнозы и цикл повторяется.

Итеративный метод оптимизации
Модели, которые работают лучше, имеют больший вес в окончательной модели ансамбля.

```Python
from sklearn.datasets import load_breast_cancer
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import train_test_split
from sklearn.ensemble import AdaBoostClassifier
data, target = load_breast_cancer(return_X_y=True)
modelClf =
AdaBoostClassifier(base_estimator=DecisionTreeClassifier(max_depth=2),
n_estimators=100, random_state=12)
X_train, X_valid, y_train, y_valid = train_test_split(data, target, test_size=0.3,
random_state=12)
modelClf.fit(X_train, y_train)
print(modelClf.score(X_valid, y_valid))
```