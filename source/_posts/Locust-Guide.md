---
title: Guide of a performance testing framework: Locust
date: 2017-03-30 15:20:45
tags:

	- Locust
	

categories: Locust

---
# What is Locust? #
Locust is an easy-to-use, distributed, user load testing tool. It is intended for load-testing web sites (or other systems) and figuring out how many concurrent users a system can handle.

# Installation in Windows#
1. First of all, you need to install python in your PC. Locust supports Python 2.7, 3.3, 3.4, 3.5, and 3.6. [→ Portal of installing python](https://www.python.org/)

2. The latest version of Locust is 0.8a2 while I was writing this article. A new page named "Charts" add to web UI in this version. Below is the most recommended way to install locust:
`pip install git+https://github.com/locustio/locust.git@master#egg=locustio`
You can see the Charts in web UI directly if you install locust by this way.
Also you can install it by `pip install locustio==0.8a2` which mentioned in Locust official website. But you will miss the Charts page. You should download the source code from [gitHub](https://github.com/locustio/locust), navigate to the root path and recompile it by the command `python setup.py install`

3. You can check whether the locust installed successfully by the command `locust --verion`.


# Quick Start #