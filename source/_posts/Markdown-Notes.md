---
title: Markdown Notes
date: 2018-12-27 22:48:22
tags: [Markdown]
categories: [Markdown]
description:
keywords:
---

# Specify your code with fenced code blocks

	```c++
	#include<iostream>
	using namespace std;
	int main(){
		cout<<"Hello World!"<<endl;
		return 0;	
	}
	```

<!-- more  -->

# Tables

	- syntax
	
	```
	| Default aligned | Left aligned | Center aligned  | Right aligned  |
	|-----------------|:-------------|:---------------:|---------------:|
	| First body part | Second cell  | Third cell      | fourth cell    |
	| Second line     | foo          | **strong**      | baz            |
	| Third line      | quux         | baz             | bar            |
	```

	- Display

| Default aligned | Left aligned | Center aligned  | Right aligned  |
|-----------------|:-------------|:---------------:|---------------:|
| First body part | Second cell  | Third cell      | fourth cell    |
| Second line     | foo          | **strong**      | baz            |
| Third line      | quux         | baz             | bar            |

# Tags	

- Note Tage Usage
```bash
/**
 * note.js | global hexo script.
 *
 * ATTENTION! No need to write this tag in 1 line if u don't want see probally bugs.
 *
 * Usage:
 *
 * {% note [class] %}
 * Any content (support inline tags too).
 * {% endnote %}
 *
 * [class] : default | primary | success | info | warning | danger.
 *           May be not defined.
 */
```
- Lable tag
```
From {% label @fairest creatures %} we desire increase
That thereby {% label primary@beauty's rose %} might never die
But as the {% label success@riper %} should by time decease,
His tender heir might {% label info@bear his memory %}
But thou contracted to thine own bright eyes
Feed'st thy light's flame with *{% label warning @self-substantial fuel%}*
Making a famine where ~~{% label default @abundance lies %}~~
Thy self thy foe, to thy <mark>sweet self too cruel</mark>
Thou that art now the world's fresh ornament
And only herald to the gaudy spring
Within thine own bud buriest thy content
And {% label danger@tender churl mak'st waste in niggarding %}
Pity the world, or else this glutton be,
{% label warning@To eat the world's due, by the grave and thee %}
```
	
From {% label @fairest creatures %} we desire increase
That thereby {% label primary@beauty's rose %} might never die
But as the {% label success@riper %} should by time decease
His tender heir might {% label info@bear his memory %}
But thou contracted to thine own bright eyes
Feed'st thy light's flame with *{% label warning @self-substantial fuel%}*
Making a famine where ~~{% label default @abundance lies %}~~
Thy self thy foe, to thy <mark>sweet self too cruel</mark>
Thou that art now the world's fresh ornament
And only herald to the gaudy spring
Within thine own bud buriest thy content
And {% label danger@tender churl mak'st waste in niggarding %}
Pity the world, or else this glutton be
{% label warning@To eat the world's due, by the grave and thee %}

# Excel Tag

```markdown
{% tabs first name %}
<!-- tab -->
**Tab 1.**
<!-- endtab -->
<!-- tab -->
**Tab 2.**
<!-- endtab -->
<!-- tab -->
**Tab 3.**
<!-- endtab -->
{% endtabs %}
```
{% tabs first name %}
<!-- tab -->
**Tab 1.**
<!-- endtab -->
<!-- tab -->
**Tab 2.**
<!-- endtab -->
<!-- tab -->
**Tab 3.**
<!-- endtab -->
{% endtabs %}

# Insert Formula

	- Syntax 

	```
	$$E=mc^2$$
	```

	- display

	$$E=mc^2$$
---------------

# References

- [Learning-Markdown](http://xianbai.me/learn-md/index.html)
- [Vincent Qin](https://www.vincentqin.tech/posts/learning-Markdown/)
- [MARKDOWN SYNTAX](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf)
