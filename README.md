## 這是django的比較好的實踐的架構。
## 增加log資料夾
## 注意：由於app集中於apps資料夾下，所以使用python manage.py startapp 要加上資料夾的位置，而且該資料夾要先建。

說明如下：
usage: manage.py startapp [-h] [--version] [-v {0,1,2,3}]
                          [--settings SETTINGS] [--pythonpath PYTHONPATH]
                          [--traceback] [--no-color] [--template TEMPLATE]
                          [--extension EXTENSIONS] [--name FILES]
                          name [directory]

Creates a Django app directory structure for the given app name in the current
directory or optionally in the given directory.

```sh
(dj110) $ python manage.py startapp newapp apps
CommandError: already exists, overlaying a project or app into an existing directory won\'t replace conflicting files
(dj110) $ python manage.py startapp newapp apps/newapp
CommandError: Destination directory does not exist, please create it first.
```