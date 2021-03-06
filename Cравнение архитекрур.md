**YOLO**

Это сеть для распознавания объектов. Её отличие от других популярных архитектур – это скорость. Это значит, что мы можем распознавать объекты в реальном времени. YOLO преобразовала задачу распознавания объектов к единой задаче регрессии. Она проходит прямо от пикселей изображения до координат содержащих рамок и вероятностей классов. CNN предсказывает множество содержащих рамок и вероятности классов для этих рамок.

YOLO смотрит на изображение только один раз, поэтому все изображения разбиваются с помощью сетки на ячейки размером 𝑆∗𝑆.

Каждая ячейка отвечает за предсказание нескольких содержащих рамок и показателя уверенности для каждой из них – другими словами, это вероятность того, что данная рамка содержит объект. Если в какой-то ячейке сетки объектов нет, то очень важно, чтобы уверенность для этой ячейки была очень малой.

Каждая ячейка отвечает за предсказание вероятностей классов. Это не значит, что какая-то ячейка содержит какой-то объект, это всего лишь вероятность. Таким образом, если ячейка сети предсказывает автомобиль, это не значит, что он там есть, но это значит, что если там есть какой-то объект, то это автомобиль. Также он испытывает трудности с небольшими объектами на изображении, это можно наблюдать на примере обнаружения стада птиц. Это связано с пространственными ограничениями алгоритма.

Yolo хоть и может быстро анализировать данный ей набор данных, точность её работы довольно мала, это можно заметить в те моменты когда нужно распознать достаточно маленький объект, в нашем случае это опухоли, которые по началу достаточно малы по объему, когда у нашей архитектуры при обучении точность возрастает и она способна определять малые по объему объекты, хоть и на её обучение требуется время. При работе с точными дисциплинами, в нашем случае медицина, это более подходящий вариант.

**Faster R-CNN**

Оба предшествующих ему алгоритма (R-CNN и Fast R-CNN) используют выборочный поиск, чтобы найти предложения региона. Выборочный поиск - это медленный и длительный процесс, влияющий на производительность сети. Поэтому был предложен алгоритм обнаружения объектов, который исключает алгоритм выборочного поиска и позволяет сети изучать предложения по регионам. Подобно Fast R-CNN, изображение предоставляется в качестве входа в сверточную сеть, которая предоставляет сверточную карту признаков. Вместо того чтобы использовать алгоритм выборочного поиска на карте объектов для идентификации предложений по регионам, для прогнозирования предложений по регионам используется отдельная сеть. Предсказанные области предложений затем изменяются, который позже используется для классификации изображения в пределах предлагаемой области и прогнозирования значений смещения для ограничивающих рамок.

Итак если сравнивать эти три версии, то FasterR-CNN быстрее чем его предшественники. Именно поэтому его можно использовать в режиме реального времени. Первые версии не имели такой возможности, из-за большого временного интервала для анализа.

Хоть и FasterR-CNN является достаточно быстрым, проблема с производительностью все еще сохранилась, в то время как sequential CNN не занимает лишнее время на обучение, что существенно важно для нашей области.

**DenseNet** 

Предполагает, что укороченное соединение в CNN позволяет обучать более глубокие и точные модели. Важно отметить, что, в отличие от ResNet, признаки прежде чем они будут переданы в следующий слой не суммируются, а конкатенируются в единый тензор. При этом количество параметров сети DenseNet намного меньше, чем у сетей с такой же точностью работы. Считается, что DenseNet работает особенно хорошо на малых наборах данных.

Для цели нашего проекта подразумевается использовать большие объемы данных, что может негативно сказаться на производительности DenseNet, что повлечет за собой неудовлетворительный результаты, а в следствии потери времени, что является ключевым ресурсом в нашем проекте.

**ThunderNet**

Использует 320 × 320 пикселей в качестве входного разрешения сети. Общая сетевая структура разделена на две части: магистральная часть и часть обнаружения.

Чтобы ускорить вывод, используются входные изображения размером 320 \* 320, также разрешение на входе должно соответствовать возможностям магистральной сети. Ни малая магистраль с большим входом, ни большая магистраль с малым входом не являются лучшим выбором.

Магистральная сеть должна обладать очень большим принимающим полем. Также информация о положении мелких объектов обширна, а глубокие объекты более различимы, поэтому необходимо учитывать обе особенности. Обычные легковесные сети нарушают вышеуказанные принципы, такие как ShuffleNetV1 / V2, что ограничивает поле опыта. ShuffleNetV2 и MobileNetV2 лишены мелких функций, в то время как Xception не имеет глубоких функций при низком вычислительном бюджете. Поэтому стоит рассматривать лишь три подходящие сети: SNet49, SNet146, SNet535.

ThunderNet с использованием SNet49 обеспечивает точность на уровне MobileNet-SSD, что почти в пять раз быстрее. По сравнению с легкой одноэтапной сетью обнаружения, ThunderNet, использующий SNet146, имеет более высокую точность. ThunderNet, использующий SNet535, сопоставим с крупномасштабными сетями обнаружения, и вычислительные затраты значительно снижаются.

При работе с нашей архитектурой, не наблюдается острой нехватки таких важных параметров как точность, производительность и скорость. При этом в ThunderNet придется выбирать какой показатель будет преобладающим.

**Заключение**

Несмотря на рассмотренные нами сети несомненно найдутся те, которые будут превосходить sequential CNN по всем определяющим важным параметрам, как время и точность определения, при этом производительность не должна страдать, но по легкодоступности и некой универсальности её работоспособности, мы сочли её подходящей для нашей цели.