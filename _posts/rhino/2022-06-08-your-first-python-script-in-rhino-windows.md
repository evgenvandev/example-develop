---
layout: post
title: "Ваш первый скрипт на Python в Rhino"
date: 2022-06-08 05:05:00
tags: python rhino
description: Скрипт на Python, отображающий окно сообщения в Rhino с надписью "Hello World"
toc: true
---

# Ваш первый скрипт на Python в Rhino (Windows)

> Оригинальная статья из документации [Your First Python Script in Rhino (Windows)](https://developer.rhino3d.com/guides/rhinopython/your-first-python-script-in-rhino-windows/)

Пример показывает как отобразить окно сообщения в Rhino с надписью "Hello World".

## Запуск скрипта в редакторе кода Rhino Python Editor

Полный путь к файлу _C:\Users\vangula\AppData\Roaming\McNeel\Rhinoceros\5.0\scripts\hello_world.py_

>hello_world.py
{:.filename}
{% highlight python %}
# coding: utf-8
import rhinoscriptsyntax as rs

rs.MessageBox("Hello World")
{% endhighlight %}

Чтобы протестировать скрипт:

- Запустите Rhino
- В командной строке введите `EditPythonScript` и нажмите `Enter`

![Запуск редактора кода Rhino Python Editor]({{'/assets/images/your-first-python-script-in-rhino-windows/type_edit-python-script.jpg' | relative_url}}){:.center-image}*Запуск редактора кода Rhino Python Editor*
- Появится диалоговое окно "Rhino Python Editor"

![Редактор кода Rhino Python Editor]({{'/assets/images/your-first-python-script-in-rhino-windows/rhino-python-editor-start.jpg' | relative_url}}){:.center-image}*Редактор кода Rhino Python Editor*
- В окне ввода кода скрипта введите приведённый выше пример кода.
- Нажмите кнопку "Запустить скрипт" (_Start Debugging_) или `F5`{: .key}

![Запуск скрипта]({{'/assets/images/your-first-python-script-in-rhino-windows/start-script.jpg' | relative_url}}){:.center-image}*Запуск скрипта*
- Диалоговое окно "Rhino Python Editor" исчезнет, и появится следующее сообщение:
![Сообщение Hello World]({{'/assets/images/your-first-python-script-in-rhino-windows/show-message-box-hello-world.jpg' | relative_url}}){:.center-image}*Сообщение Hello World*

## Отображение русских букв при выполнении скрипта

Для отображения букв русского алфавита, необходимо писать исходный код скрипта в кодировке `utf-8`. Чтобы сообщить интерпретатору _Python_, что файл с исходным кодом скрипта использует кодировку `utf-8`, пишем в первой или второй строке файла с исходным кодом комментарии, 

{% highlight python %}
# coding: utf-8
{% endhighlight %}

указывающий какую кодировку нужно использовать при чтении файла.

Также можно использовать следующий комментарии:

{% highlight python %}
# This Python file uses the following encoding: utf-8
{% endhighlight %}

Информация взята с сайта: [Определение кодировок исходного кода Python](https://peps.python.org/pep-0263/#examples)

## Функция HelloWorld()

Если бы вы писали более сложный скрипт и хотели отображать "Hello World" в стратегических точках по всему скрипту, вы могли бы писать этот код каждый раз, когда вы хотите, чтобы сообщение появлялось.

Но если вы передумаете и захотите вместо этого сказать "Howdy World", вам придётся искать все места, где использовалось "Hello World", и заменять их.

Более простой способ решить эту проблему - написать функцию (`def` используется для определения функции). В нескольких местах скрипта вы вызываете функцию. Функция выполняется, отображая сообщение, поэтому вам нужно изменить текст сообщения только в одном месте.

Вот как выглядит определение функции:

>hello_world.py
{:.filename}
{% highlight python %}
# coding: utf-8
import rhinoscriptsyntax as rs

def HelloWorld():
	rs.MessageBox("Hello World")
{% endhighlight %}

Если вы нажмёте кнопку запуска (_Start Debugging_), ничего не произойдёт. Это потому, что _RhinoScript_ определил подпрограмму, но фактически не вызывал её. Чтобы вызвать подпрограмму, добавьте следующую строку кода и нажмите _Start Debugging_.

{% highlight python %}
HelloWorld()
{% endhighlight %}

В _Python_ определения функций должны появиться до того, как они будут вызваны в коде.

Полный код примера:

Полный путь к файлу _C:\Users\vangula\AppData\Roaming\McNeel\Rhinoceros\5.0\scripts\hello_world.py_

>hello_world.py
{:.filename}
{% highlight python %}
# coding: utf-8
import rhinoscriptsyntax as rs

def HelloWorld():
	rs.MessageBox("Hello World")

HelloWorld()
{% endhighlight %}

Когда вы делаете больше, чем пишете пару строк скрипта, возникает необходимость сохранить скрипт в файл, чтобы вы могли запускать скрипт, не набирая его каждый раз.

### Сохранить код скрипта

В диалоговом окне "Rhino Python Editor" нажмите кнопку "Save". Сохраните файл, как _"hello_world.py"_.

### Запуск hello_world.py

- В командной строке введите `RunPythonScript` и нажмите Enter.
- В диалоговом окне "Run Python Script" выберите "hello_world.py" и нажмите "Open".

Ваш скрипт запустится и отобразит знакомое диалоговое окно "Hello World".