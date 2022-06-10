---
layout: post
title: "Использование RhinoCommon из Python"
date: 2022-06-10 05:05:00
tags: python rhino
description: Наряду с функциями rhinoscriptsyntax вы можете использовать все классы .NET Framework, включая классы, доступные в RhinoCommon.
toc: true
---

# Использование RhinoCommon из Python

> Оригинальная статья из документации [Using RhinoCommon from Python](https://developer.rhino3d.com/guides/rhinopython/using-rhinocommon-from-python/)

Наряду с функциями _rhinoscriptsyntax_ вы можете использовать все классы _.NET Framework_, включая классы, доступные в _RhinoCommon_. На самом деле, если вы посмотрите на исходный код _rhinoscriptsyntax_, то увидите, что это просто скрипты Python, использующие _RhinoCommon_. Это позволяет вам делать довольно удивительные вещи внутри скрипта Python. Многие функции, которые когда-то можно было реализовать только в плагине _.NET_, теперь можно реализовать в скрипте Python.

Например, вы можете реализовать некий пользовательский рисунок, пока пользователь выбирает точку, с помощью следующего скрипта. Этот сценарий рисует красную и синюю линии, соединённые с точкой под курсором мыши, пока пользователь выбирает точку.

>image_dynamic_draw_under_cursor.py
{:.filename}
{% highlight python %}
# coding: utf-8
import Rhino
import System.Drawing

def GetPointDynamicDrawFunc( sender, args ):
    pt1 = Rhino.Geometry.Point3d(0, 0, 0)
    pt2 = Rhino.Geometry.Point3d(10, 10, 0)
    args.Display.DrawLine(pt1, args.CurrentPoint, System.Drawing.Color.Red, 2)
    args.Display.DrawLine(pt2, args.CurrentPoint, System.Drawing.Color.Blue, 2)

# Creating an instance of a GetPoint class and add a delegate for the DynamicDraw event
# Создание экземпляра класса GetPoint и добавление делегата для события DynamicDraw
gp = Rhino.Input.Custom.GetPoint()
gp.DynamicDraw += GetPointDynamicDrawFunc
gp.Get()
{% endhighlight %}

Результат работы программы:

![Динамический рисунок под курсором - вид сверху]({{'/assets/images/using-rhinocommon-for-python/image-dynamic-draw-under-cursor_top-view.jpg' | relative_url}}){:.center-image}*Динамический рисунок под курсором - вид сверху*

![Динамический рисунок под курсором - вид перспектива]({{'/assets/images/using-rhinocommon-for-python/image-dynamic-draw-under-cursor_perspective-view.jpg' | relative_url}}){:.center-image}*Динамический рисунок под курсором - вид перспектива*
