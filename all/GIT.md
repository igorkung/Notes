Из под пользователя: 
Проверка конфига (всего и глобального и логкального): 
- локальный может хранится внутри рабочей папки
```
$ git config --list
```
Внести изменения в конфиг:
```
$ git config --local user.name "John Doe"
$ git config --global user.email johndoe@example.com
```