### Загрузить vdh-образ на ya.cloud

###### 1. Создать vdh-образ из виртуальной машины VBox
```



```
***
###### 2. В YandexCloud создать ключ
`IAM -> "Создать статический ключ" -> Сохранить AccesKey и SecretKey`
***
###### 3. Установить s3cmd на хостовой компьютер

> Должен быть установлен python

Установка:
```python
pip install s3cmd
```
Использование утилиты:
```python
python "$env:USERPROFILE\AppData\Roaming\Python\Python313\Scripts\s3cmd"
```

Мануал использования s3cmd
***
###### 4. Настроить конфигурацию s3cmd
```python
python "$env:USERPROFILE\AppData\Roaming\Python\Python313\Scripts\s3cmd" --configure
```
```python
# Примерная конфигурация:
New settings:
  Access Key: YCAJEXv75s-IGQgsU1EnTVPs_
  Secret Key: YCPqP1gJEizgeeMAM8jpDRPaUoTspjFf8Vs3Qnw3
  Default Region: ru-central1
  S3 Endpoint: storage.yandexcloud.net
  DNS-style bucket+hostname:port template for accessing a bucket: %(bucket)s.storage.yandexcloud.net
  Encryption password:
  Path to GPG program: None
  Use HTTPS protocol: True
  HTTP Proxy server name:
  HTTP Proxy server port: 0
```
###### 5. Отправить vdh-образ в бакет
```python
python "$env:USERPROFILE\AppData\Roaming\Python\Python313\Scripts\s3cmd" put "C:\Users\Asus TUF Gaming\Desktop\windows10.vhd" s3://ssoi-backet/
```