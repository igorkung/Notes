#### Получает урл откуда данные забрать и парсит json. Скрипт делелся под AGI астера есть логирование. 

```
#!/usr/bin/python3

###/usr/bin/python3 
###/var/lib/asterisk/agi-bin/customer_order/venv/bin/python
 
import sys
import requests
import json
import traceback
import argparse

#from datetime import datetime, timezone

##==== переменные собюирающие данные о фаиле скрипта и каталоге запуска
import os
script_dir = os.path.dirname(os.path.abspath(__file__))
log_file = os.path.join(script_dir, __file__ + '.log')

##===== Логгер + ротация ===== 
import logging
import logging.handlers
logger = logging.getLogger(__name__)
logger.setLevel(logging.INFO) 
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s', datefmt='%Y-%m-%d %H:%M:%S')
handler = logging.handlers.TimedRotatingFileHandler( ## хендлер ротации логов
    log_file,  # Имя файла
    when='midnight',  # Ротация каждый день
    interval=1,  # Интервал в днях
    backupCount=7,  # Хранить логи за неделю
    encoding='utf-8' # Кодировка
)
handler.setFormatter(formatter) 
logger.addHandler(handler) ## включаем хэндлер
##==================


## функция для запросов общая
def  url_get_json(url,jsonvar):
    headers = {
        "Content-Type": "application/json",
    }
    # запрос
    logger.info('Старт попытки запроса')
    try: 
        response = requests.get(url=url, headers=headers)
        print(response.status_code)
    except ValueError as e:
        logger.info(f"Request ERROR: {e}")
    logger.info(response.status_code)


    if response.status_code == 200:
#        logger.info('Код 200 ')
        try:
            jsondata = response.json() # Parse the JSON response
            if jsonvar == 'all':
                logger.info(f"jsondata = {jsondata}")
                return jsondata
            else: 
                jsonvariable = jsondata.get(jsonvar)  # Safely access jsonvar
                logger.info(f"jsonvariable = {jsonvariable}")
                return jsonvariable

        except ValueError as e:
            logger.info(f"Response is not valid JSON: {e}")
            #print("Response is not valid JSON")
            return "None"
    else:
        logger.warning(f"Request failed with status code {response.status_code}; url_get_json( url={url}, jsonvar={jsonvar})")
#        print(f"Request failed with status code {response.status_code}")
        return response.status_code
## ====================================


def parse_arguments():
    """Парсинг аргументов командной строки"""
    parser = argparse.ArgumentParser(description='Обработка запросов по URL')
    parser.add_argument('url', help='url - откуда дергаем данные')
    parser.add_argument('jsonvar', nargs='?', default='all', help='Переменная для поиска в выводе, если не определена выводит все')
    return parser.parse_args()

# Base start
if __name__ == "__main__":
    logger.info(' ===== START program ===== ')
#    args = parse_arguments()  # парсинг переданных данных 

    # Получение номера телефона из аргументов командной строки
    if len(sys.argv) > 1:
        arg_url = sys.argv[1]
        arg_jsonvar = sys.argv[2]
        logger.info(f'input variable: url - {arg_url} | jsonvar - {arg_jsonvar}')

    else:
        logger.info(f'Данных нет выходим')
        sys.exit()

 
    reasult = url_get_json(arg_url, arg_jsonvar)
    logger.info(f'Reasult from uri: {reasult}')
    print(f"SET VARIABLE VarFromUri {reasult}")
    logger.info(' ===== END program ===== ')
#    print(reasult)
```
