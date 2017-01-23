## 這是django的比較好的實踐的架構。
## 要增加log資料夾在project 資料夾，及apps資料夾下。
### 注意：由於目的是將app集中於apps資料夾下，所以在apps資料夾下用`python ..manage.py startapp newapp`。(使用預設python manage.py startapp 如加上資料夾的位置，而且該資料夾要先建，而且名字不能一樣。)

## djgunicorn是給本機開發時使用的，並未放入base.py。 

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
此外，您會需要一個.env檔，但由於安全性的關係，我將其置於下方

`.env`

```
DEBUG=true
SECRET_KEY="你自己的key, Change to your SECRET_KEY。"
DB_NAME=my_db
DB_USER=postgres
DB_PASS=postgres
DB_SERVICE=172.17.0.1
DB_PORT=5432
```
set_env.sh 會啟動一個postgres sql資料庫的docker container，你得把它改成你的，如果是資料庫是新的，你還得進到docker container去建一個叫'my_db'的資料庫。

$ docker pull postgres:latest #取得postgresql的最新版的docker images
$ docker run -P postgres:latest #開啟一個postgresql的container
$ docker ps #查看連接的port
$ docker ps
CONTAINER ID    IMAGE            COMMAND                  CREATED             STATUS              PORTS                       NAMES
3d902da36c1e    postgres:latest  "/docker-entrypoint.s"   33 seconds ago      Up 32 seconds       0.0.0.0:32768->5432/tcp     condescending_boyd

$ psql -h localhost -p 32768 -U postgres

# psql --help #看psql的說明。
# CREATED DATABASE my_db;
# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-----------+----------+----------+------------+------------+-----------------------
 my_db     | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 

WARNING: No password has been set for the database.
         This will allow anyone with access to the
         Postgres port to access your database. In
         Docker's default configuration, this is
         effectively any other container on the same
         system.

         Use "-e POSTGRES_PASSWORD=password" to set
         it in "docker run".

以下為範例，取自http://margaine.com/2015/04/29/common-lisp-and-docker.html
最後的postgres就是指image。
The db container

Here is the command to create the db container:

$ sudo docker run \
    --volume=/var/lib/postgresql/data \
    --env="POSTGRES_PASSWORD=password" \
    --name=db postgres

docker參考資料
https://davidamick.wordpress.com/2014/07/19/docker-postgresql-workflow/
https://www.stavros.io/posts/how-deploy-django-docker/
https://docs.docker.com/engine/examples/postgresql_service/

## logging 日誌功能
Python 的logging 配置由四個部分組成：

    - 記錄器(logger)
    - 處理程序(handler)
    - 過濾器(filter)
    - 格式化(formatter)

### 記錄器(logger)
Logger 為日誌系統的入口。每個logger命名都是bucket，你可以向這個bucket寫入需要處理的消息。

### 處理程序(handler)
Handler 決定如何處理logger 中的每條消息。它表示一個特定的日誌行為，例如將消息寫到屏幕上、寫到文件中或者寫到網絡socket。

### 過濾器(filter)
Filter 用於對從logger 傳遞給handler 的日誌記錄進行額外的控制。

### 格式化(formatter)
最後，日誌記錄需要轉換成文本。Formatter 表示文本的格式。Fomatter 通常由包含日誌記錄屬性的Python 格式字符串組成；你也可以編寫自定義的fomatter 來實現自己的格式。

### using logger
you need to place logging calls into your code. Using the logging framework is very simple. Here’s an example:
配置好logger、handler、filter 和formatter 之後，你需要在代碼中放入logging 調用。使用logging 框架非常簡單。下面是個例子：

```python
# import the logging library
import logging

# Get an instance of a logger
logger = logging.getLogger(__name__)

def my_view(request, arg1, arg):
    ...
    if bad_mojo:
        # Log an error message
        logger.error('Something went wrong!')
```


日誌功能參考資料
https://github.com/uranusjr/django-tutorial-for-programmers/blob/master/28-logging-data-migration-and-media-files.md
http://zwindr.blogspot.tw/2016/08/python-logging.html
http://python.usyiyi.cn/python_278/library/logging.html
http://python.usyiyi.cn/translate/django_182/topics/logging.html
