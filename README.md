## 這是django的比較好的實踐的架構。
## 要增加log資料夾在project 資料夾，及apps資料夾下。
### 注意：由於目的是將app集中於apps資料夾下，所以在apps資料夾下用`python ..manage.py startapp newapp`。(使用預設python manage.py startapp 如加上資料夾的位置，而且該資料夾要先建，而且名字不能一樣。)


說明如下：
usage: manage.py startapp [-h] [--version] [-v {0,1,2,3}]
                          [--settings SETTINGS] [--pythonpath PYTHONPATH]
                          [--traceback] [--no-color] [--template TEMPLATE]
                          [--extension EXTENSIONS] [--name FILES]
                          name [directory]

Creates a Django app directory structure for the given app name in the current
directory or optionally in the given directory.

If the optional destination is provided, Django will use that existing directory rather than creating a new one. You can use ‘.’ to denote the current working directory.

And if the optional destination is provided, the name of existing directory must diff from app name.

```sh
(dj110) $ python manage.py startapp newapp apps
CommandError: already exists, overlaying a project or app into an existing directory won\'t replace conflicting files
(dj110) $ python manage.py startapp newapp apps/newapp
CommandError: Destination directory does not exist, please create it first.
```