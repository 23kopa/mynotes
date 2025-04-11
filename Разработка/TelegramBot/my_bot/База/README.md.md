Для установки бота необходимо разархивировать файлы бота в директорию на локальном 
компьютере и использовать команды для поднятия докера:

	#Собираем образ
	docker build -t <IMAGE-NAME> .
	
	#Запускаем контейнер
	docker run -d --name <COUNTAINER-NAME> <IMAGE-NAME>


Для пересборки бота используйте следующие команды: 

	#Остановка контейнера
	docker stop <COUNTAINER-NAME>
	
	#Удаление контейнера (опционально)
	docker rm <COUNTAINER-NAME>
	
	#Удаление образа
	docker rmi <IMAGE-NAME>
	
	#Вносим изменеия
	
	#Собираем образ
	docker build -t <IMAGE-NAME> .
	
	#Запускаем контейнер
	docker run -d --name <COUNTAINER-NAME> <IMAGE-NAME>