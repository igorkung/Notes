1111
пока пишу все сюда в дальнейшем думаю разобью 

Python
```
# Получаем путь к текущему скрипту
log_file = os.path.join(script_dir, '{имя фаила с логами}.log')
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
    response = requests.get(url=url, headers=headers)
    #print(response.status_code)

    if response.status_code == 200:
        try:
            jsondata = response.json()  # Parse the JSON response
            jsonvariable = jsondata.get(jsonvar)  # Safely access jsonvar

            # Принт всего для тестов
            #print(jsondata)

            return jsonvariable
        except ValueError:
            logger.warning("Response is not valid JSON")
            #print("Response is not valid JSON")
            return "None"
    else:
        logger.warning(f"Request failed with status code {response.status_code}; url_get_json( url={url}, jsonvar={jsonvar})")
#        print(f"Request failed with status code {response.status_code}")
        return response.status_code
## ====================================
```
