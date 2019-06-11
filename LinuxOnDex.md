# install

# config
- langauage pack
```
sudo apt-get install language-pack-kr
``` 
- fcitx
```
sudo apt-get install fcitx fcitx-hangul
```
- run fcitx and exit
```
fcitx
....
and then 'ctrl+c'
```
- execute fcitx-config-gtk3
```
fcitx-config-gtk3
```
- click '+' -> Search 'Hangule' and add -> global config -> set hot key
- add environment parameter
```
vi .bashrc
```
  - add command at tail and save 
  ```
  /usr/bingnome-terminal --command /usr/bin/fcitx
  ```
  ```
  source .bashrc
  ```
  - reboot dex
  
