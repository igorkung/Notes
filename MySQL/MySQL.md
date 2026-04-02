Тут планирую собирать пока заметки по мусклю и запросам 
бызовый набор: 
#SELECT VERSION(); 
#SHOW PROCESSLIST;
#SELECT * FROM mysql.user;
#SELECT * FROM mysql.db;
#CREATE USER 'user'@'localhost' IDENTIFIED BY 'passsssss';
#SET PASSWORD FOR 'user'@'localhost' = 'pass';
#UPDATE mysql.db SET host='%' WHERE Host='10.0.0.0' AND User='freepbxuser';
#UPDATE mysql.user SET host='%' WHERE Host='10.0.0.0' AND User='freepbxuser';
#GRANT select, update, insert, delete ON asteriskcdrdb.* to 'DBview'@'%'; 
#GRANT ALL ON asteriskcdrdb.* TO 'freepbxuser'@'10.0.0.%';
#FLUSH privileges;

coolation: 
приколотить новый: SET GLOBAL default_collation_for_utf8mb4 = 'utf8mb4_general_ci'; -- времено до рестарта
                   SET PERSIST -- не проверия


процедуры:
