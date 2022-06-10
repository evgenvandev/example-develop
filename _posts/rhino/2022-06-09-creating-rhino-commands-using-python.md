---
layout: post
title: "Создание команд Rhino с использованием Python"
date: 2022-06-09 05:05:00
tags: python rhino
description: Создание команд Rhino из скриптов Python
toc: true
---

# Создание команд Rhino с использованием Python

> Оригинальная статья из документации [Creating Rhino Commands Using Python](https://developer.rhino3d.com/guides/rhinopython/creating-rhino-commands-using-python/)

Можно, если немного разобраться, создать "настоящие" команды Rhino из скриптов Python. Ниже описана процедура для Rhino в Windows с несколькими комментариями о том, куда файлы должны идти, чтобы всё это работало с Rhino в Mac. Исходный шаблон для команд вместе с новым идентификатором _GUID_ и соответствующими папками автоматически создаются редактором скриптов "Rhino Python Editor" в Windows.

## В Windows

1. Запустите редактор "Rhino Python Editor" с помощью команды `EditPythonScript`.
1. Нажимаем _New script_ и выбираем пункт _Command_ в следующем диалоговом окне.
![Выбор команды из диалогового окна]({{'/assets/images/creating-rhino-commands-using-python/creating-rhino-commands-using-python-01.jpg' | relative_url}}){:.center-image}*Выбор команды из диалогового окна*
1. Введите имя команды (_MyNewCommand_) и имя плагина (_MyNewPlug-In_), в котором вы хотели бы использовать команду - плагин может быть новым или уже существующим. Вы можете добавить несколько команд в плагин. В этом примере мы использовали _MyNewCommand_ в _MyNewPlugIn_. Создаётся новый файл скрипта с фреймворком для новой команды...

>MyNewCommand_cmd.py
{:.filename}
{% highlight python linenos %}
import rhinoscript.userinterface
import rhinoscript.geometry

__commandname__ = "MyNewCommand"

# RunCommand is the called when the user enters the command name in Rhino.
# The command name is defined by the filename minus "_cmd.py"
def RunCommand( is_interactive ):
  print "Hello", __commandname__
  # get a point
  point = rhinoscript.userinterface.GetPoint()
  if( point != None ):
    rhinoscript.geometry.AddPoint(point)

  # you can optionally return a value from this function
  # to signify command result. Return values that make
  # sense are
  #   0 == success
  #   1 == cancel
  # If this function does not return a value, success is assumed
  return 0
  
{% endhighlight %}

1. Код, который вызывает команда, находится в функции `def RunCommand( is_interactive ):`
1. Добавьте сюда свой код скрипта - шаблон содержит фиктивный код и пару комментариев, и все они должны вызываться из этой части скрипта. Весь скрипт может быть в этой одной функции, или вы можете добавить другие функции, как обычно, по мере необходимости. Обязательно измените начальные `import` операторы, чтобы они соответствовали скрипту, который вы пишете - автоматические операторы `import` существуют для представленного примера скрипта.
1. Добавьте эту строку внизу скрипта `RunCommand(True)`. Это позволяет вам тестировать скрипт из редактора, отлаживать и т.д. Просто не забудьте закомментировать эту строку, прежде чем распростронять скрипт или пытаться использовать его в качестве команды. Также можно написать более "обычный" скрипт, тест и т.д. по мере необходимости обычным способом, а затем просто скопировать и вставить в этот отформатированный шаблон командного скрипта.
1. Обратите внимание, что в Rhino в Windows скрипт автоматически сохраняется в специальном месте:
_c:\Users\vangula\AppData\Roaming\McNeel\Rhinoceros\5.0\Plug-ins\PythonPlugins\MyNewPlug-In {fbdf7d48-66ee-4a88-8294-caafb6b78a4b}\dev\MyNewCommand_cmd.py_
![Путь к файлу скрипта]({{'/assets/images/creating-rhino-commands-using-python/creating-rhino-commands-using-python-02.jpg' | relative_url}}){:.center-image}*Путь к файлу скрипта*
_fbdf7d48-66ee-4a88-8294-caafb6b78a4b_ - это идентификатор GUID.
чтобы Rhino знал, где искать командный скрипт. Этот путь устанавливается редактором при первоначальном создании команды.
1. _Необязательно_: чтобы распространять скрипт как плагин, просто поделитесь всей папкой. Вы можете заархивировать её, если хотите, и другие пользователи могут легко извлечь _MyNewPlug-In {fbdf7d48-66ee-4a88-8294-caafb6b78a4b}_ папку и поместить её в _%APPDATA%\McNeel\Rhinoceros\5.0\Plug-ins\PythonPlugIns\_ на свои компьютеры.

Rhino должен быть закрыт при "установке" новых плагинов, иначе его нужно будет перезапустить, прежде чем он распознает какие-либо новые команды.

Пример скрипта - выводит две линии красного и зелёного цвета, линии пересекаются, точка пересечения синего цвета.

>MyNewCommand_cmd.py
{:.filename}
{% highlight python linenos %}
# coding: utf-8
import rhinoscript.userinterface
import rhinoscript.geometry
import rhinoscriptsyntax as rs

__commandname__ = "MyNewCommand"

# RunCommand is the called when the user enters the command name in Rhino.
# The command name is defined by the filname minus "_cmd.py"
def RunCommand( is_interactive ):
  #print "Hello", __commandname__
  # get a point
  #point = rhinoscript.userinterface.GetPoint()
  #if( point != None ):
    #rhinoscript.geometry.AddPoint(point)
  
  # you can optionally return a value from this function
  # to signify command result. Return values that make
  # sense are
  #   0 == success
  #   1 == cancel
  # If this function does not return a value, success is assumed
  
  intersection_two_color_line()
  
  return 0

def intersection_two_color_line():
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
    
    print "Команда ", __commandname__, " выполнена"
{% endhighlight %}

![Результат работы скрипта]({{'/assets/images/creating-rhino-commands-using-python/result-work-script.jpg' | relative_url}}){:.center-image}*Результат работы скрипта*