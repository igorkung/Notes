<?php

## работающий запрос: http://app-site/record-file-ga.php?file=in_1750939121.22618.wav 
## решение на PHP ищет и проигрыевает фаил по ID

date_default_timezone_set('Asia/Yekaterinburg');

header('Content-Type: text/html; charset=utf-8');

## в виде id получаем имя фаила 
if (empty($_REQUEST['call_id'])) {
        echo 'Не указан идентификатор звонка';
        return;
}

#$file = $_GET['call_id'];

$id = $_GET['call_id'];


## В нашем случае придут только цифры
## outpit_id вырезаем циферки из названия фаила 
#preg_match('/\d{10}\.\d{5}/', $file, $output_id);
#$id = $output_id[0];
#print($output_id[0]);
#return;

## определяем дату для поиска попапкам
$yeah = date('Y', $id);
$month = date('m', $id);
$day = date('d', $id);

$full_dir = '/var/spool/asterisk/monitor/'.$yeah.'/'.$month.'/'.$day.'/';

#print($full_dir);
#return;

// Сканирование папки и составление списка фаилов, поиск в фаилах совпадения по ID
// Дергает первый попавшийся и воспроизводит
$files_search = scandir($full_dir);
 //Сортировка по названию (А, Б, В...)
//    sort($files);
//    print_r($files_search);
    foreach($files_search as $file_search){
//    print($file_search);
    $file_search_t='test'.$file_search;  
//    if (str_contains($file,$id)) { 
    if (strpos($file_search_t,$id)) {
//          print($file_search); 
          $recordf = $file_search;
          $wav = $full_dir.$recordf;
          if (!is_readable($wav)) {
              echo "Не удалось прочитать файл {$wav}";
              return;
          }

          if (isset($_GET["type"]) && $_GET["type"] == "download") {
              header("Content-Type: application/octet-stream");
              header("Content-Disposition: attachment; filename={$recordf}");
          } else {
              header("Content-type: audio/x-wav");
              header('Accept-Ranges: bytes');
              header('Connection: close');
              header('Content-Length: '.filesize($wav));
              header("Content-Range: bytes 0-".filesize($wav)."/".filesize($wav));
          }
          echo file_get_contents($wav);
          return;
       } else {
          $recordf = '0';
       }

    }
if ($recordf == '0' ) {
    echo 'Фаил не найден';
}

?>
