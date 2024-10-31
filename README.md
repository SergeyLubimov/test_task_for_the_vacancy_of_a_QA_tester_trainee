# ОЦЕНКА ВРЕМЕНИ
Чисто время выполнения работы оценить сложно, так как оно растянулось урывками на несколько дней. 
В выходные был вынужден отвлекаться по семейным обстоятельствам. 

Сборка, установка и нахождение ошибки по заданию 3 были выполнено 27 октября. Отчет был составлен сегодня 31 октября.
Таким образом на задания 1-4 было потрачено примерно 5 часов.

После этого несколько дней были попытки вызвать падение по Заданию 5. То и дело возвращался к этой задаче, 
когда приходила мысль в голову, но точно оценить время не могу.

# ОШИБКА НЕ ВОСПРОИЗВОДИТСЯ 
В файле Падение_сервера.pdf представлен сценарий, который на данный момент не приводит к разрыву подключения, 
однако до этого ошибка несколько раз воспроизводилась. 
Изначально она была обнаружена в 28 октября, но в другом виде:
    После удаления всех записей pg_class, попытка выполнения SELECT 1 привела к разрыву соединения и базой, 
    с невозможностью переподключится.
    
Однако воспроизвести ее получилось лишь в модифицированном виде:
    Перед очисткой pg_class я создавал новую таблицу и добавлял пару новых значений. 
    После этого SELECT 1 стабильно вызывал разрыв соединения.
Повреждение системного файла я посчитал слишком простым решением и стал искать дальше. Все дальнейшие попытки либо вызывали зависание, 
либо являлись обработанными исключениями и не вызывали падения сервера.

Поэтому 30 октября вернулся к этому сценарию, но решил его модифицировать, чтобы вызывать ошибку FATAL, а не PANIC. 
Для этого помимо очистки pg_class также очищал pg_database.
В таком виде падение воспроизводилось стабильно.

Но сегодня 31 октября, приступив к отчету по этой части, попытки воспроизвести сценарий не удавались. 
До этого сегодня у меня возникали проблемы с виртуальной машиной, мне пришлось создать новую, 
куда перекачал репозиторий и собрал постгресс заново. Я предположил, что данную ошибку исправили в одном из последних каммитов. 
Для проверки предположения к коммиту выполненному 25 октября. Однако это не помогло воспроизвести ошибку.

Полагаю ошибка связана с внутренними процессами работы сервера, которые я пока не понимаю, или зависело от неких специфичных условий. 
В то время как сценарий лишь случайно ее выявлял.

*****

На данный момент сценарий очень сильно нарушает функциональность сервера. Он практически мертв. Но подключение не прерывается. 
Если отключится вручную, то повторное подключение невозможно.
К сожалению, я не сделал скиншотов и не сохранил логи удавшихся падений – все представленные скриншоты были сделаны уже после того, 
как сценарий перестал вызывать разрыв подключения. Это было крайне неразумно с моей стороны и будет мне уроком. 
Увы мне не чем доказать то, что эти падения вообще были и изначально я и вовсе хотел отправить только задания 1-4, 
но и итоге решил все-таки рассказать о проделанной работе.
