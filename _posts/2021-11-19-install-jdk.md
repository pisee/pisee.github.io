---
title: install-jdk
permalink: /install-jdk/
---

# 1. download openjdk
https://github.com/ojdkbuild/ojdkbuild


# 2. centos7
- unzip  
- java_home  
  ```bash
  vi ~/.bash_profile
  ```  
  
  ```bash
  JAVA_HOME =/path/to/jdk/home
  PATH=$PATH:$HOME/.local/bin:$HOME/bin:$JAVA_HOME/bin
  export PATH
  ```  
  
  ```bash
  source .bash_profile
  ```  
