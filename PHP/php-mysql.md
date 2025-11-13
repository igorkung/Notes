Код для инсерта посредстовм PHP можно делать любой запрос

```php
#!/usr/bin/php

<?php
#AST-apps-asteriskcdrdb
$dbhost = "dns/ip";
$db_basa = "name DB";
$user = "Username";
$pass = "Password";

#var_dump($argv);

$conn = new mysqli($dbhost, $user, $pass, $db_basa);
if($conn->connect_error){
    die("Ошибка: " . $conn->connect_error);
}
# INSERT INTO survey (`calldate`, `src`, `dst`, `rating`, `uniqueid`, `linkedid`, `queue`, `name`) VALUES (now(), 'телефон', 'тел_опер', цифра после разговора, 'uniqueid', 'linkedid', 'очередь', 'имя оператора')
#mysqli_set_charset( 'utf-8', $conn);
$conn->set_charset("utf8mb4");
#$conn->set_charset('utf-8');
$sql = "INSERT INTO survey_test (`calldate`, `src`, `dst`, `rating`, `uniqueid`, `linkedid`, `queue`, `name`) VALUES (now(),'$argv[1]','$argv[2]',$argv[3],'$argv[4]','$argv[5]','$argv[6]','$argv[7]')";
if($conn->query($sql)){
    echo "Данные успешно добавлены";
} else{
    echo "Ошибка: " . $conn->error;
}
#echo "подключилося \n";
$conn->close();
?>
```