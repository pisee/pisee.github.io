---
title: install-jdk
permalink: /install-jdk/
---

# download openjdk
https://github.com/ojdkbuild/ojdkbuild


# centos7
1. unzip
1. java_home
  ```bash
  vi ~.bash_profile
  ```
  ```bash
  JAVA_HOME =/path/to/jdk/home
  PATH=$PATH:$HOME/.local/bin:$HOME/bin:$JAVA_HOME/bin

  export PATH

  ``` 
  ```bash
  source .bash_profile
  ```
