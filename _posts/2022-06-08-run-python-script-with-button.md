---
layout: post
title: "Запуск Python скрипта кнопкой на панели"
date: 2022-06-08 05:08:00
tags: python rhino
description: Запуск скрипта, написанного на Python, с помощью кнопки на панели инструментов.
toc: true
---

# Запуск Python скрипта кнопкой на панели

После того, как скрипт написан и протестирован, вы можете поместить его в легкодоступное место, например, на кнопку панели инструментов _Rhino_.

![Пример кнопки "MyCommand"]({{'/assets/images/run-python-script-with-button/example-run-button.jpg' | relative_url}}){:.center-image}*Пример кнопки "MyCommand"*

## Создаём панель инструментов в Rhino

На любой панели инструментов нажимаем правую кнопку мыши и выбираем _New Tab_.

![Выбираем New Tab]({{'/assets/images/run-python-script-with-button/select-new-tab.jpg' | relative_url}}){:.center-image}*Выбираем New Tab*

Меняем название панели с _Toolbar_ на _PythonButton_.

## Создаём кнопку на панели инструментов в Rhino

На получившейся панели нажимаем правую кнопку мыши и выбираем _New Button..._.

![Выбираем New Button...]({{'/assets/images/run-python-script-with-button/select-new-button.jpg' | relative_url}}){:.center-image}*Выбираем New Button...*

Открывается диалоговое окно "Button Editor":

![Настраиваем кнопку]({{'/assets/images/run-python-script-with-button/button-editor-settings-button.jpg' | relative_url}}){:.center-image}*Настраиваем кнопку*

1. Отмечаем, чтобы на кнопке отображалась только картинка (_Image only_). Картинку создадим в пункте 3.
1. Вводим название команды, которое будет отображаться - _MyCommand_
1. Создаём картинку для кнопки.
1. В поле _Tooltip_ вводим текст всплывающей подсказки - IntersectionTwoLine
1. В поле _Command_ вводим команду 
```
!-_RunPythonScript (path_to_file)
```
где `path_to_file` - путь к файлу скрипта.
В нашем примере путь к файлу следующий:
`"c:\Users\vangula\AppData\Roaming\McNeel\Rhinoceros\5.0\scripts\intersection_two_line.py"`
```
!-_RunPythonScript ("c:\Users\vangula\AppData\Roaming\McNeel\Rhinoceros\5.0\scripts\intersection_two_line.py")
```

## Скрипт Python применяемый в примере

Полный путь к файлу _C:\Users\vangula\AppData\Roaming\McNeel\Rhinoceros\5.0\scripts\intersection_two_line.py_

>intersection_two_line.py
{:.filename}
{% highlight python %}
# coding: utf-8
import rhinoscriptsyntax as rs

startPoint = [1.0, 2.0, 0.0]
endPoint = [4.0, 5.0, 0.0]
line1 = [startPoint, endPoint]

# Adds a line to the Rhino Document and returns an ObjectID
# Добавляет линию в документ Rhino и возвращает 
# идентификатор объекта ObjectID
line1ID = rs.AddLine(line1[0], line1[1])

# Устанавливаем цвет для объекта - красный
rs.ObjectColor(line1ID, [255, 0, 0])

startPoint2 = [1.0, 4.0, 0.0]
endPoint2 = [4.0, 2.0, 0.0]
line2 = [startPoint2, endPoint2]

# Returns another ObjectID
# Возвращает другой идентификатор объекта ObjectID
line2ID = rs.AddLine(line2[0], line2[1])

# Устанавливаем цвет для объекта - зелёный
rs.ObjectColor(line2ID, [0, 255, 0])

# Passing the ObjectIDs to the function
# Передаём идентификаторы объектов (линий) в другую функцию
int1 = rs.LineLineIntersection(line1ID, line2ID)

if int1:
    # Получаем идентификатор объекта 
    # для точки пересечения линии
    int1ID = rs.AddPoint(int1[0])
    # Устанавливаем цвет для объекта - синий
    rs.ObjectColor(int1ID,  [0, 0, 255])
    #rs.AddPoint(int1[1])

print("Команда выполнена")
{% endhighlight %}

В результате получим две пересекающиеся линии, красного и зелёного цвета и точку их пересечения синего цвета.

![Результат работы скрипта]({{'/assets/images/run-python-script-with-button/result-work-script.jpg' | relative_url}}){:.center-image}*Результат работы скрипта*
