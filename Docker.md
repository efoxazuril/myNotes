# Docker

## Основные комманды

 ```bash
sudo usermod -aG docker $USER #добавить текущего пользователя в группу docker
 ```

```bash
docker <имя_команды> --help # получить справку
docker ps 					#покажет список работающих контейнеров
docker ps -a 				# покажет список всех контейнеров
docker search <имя_образа> 	# поищет на hub.docker.com образ
docker run <имя_образа> 	# запустит данный образ
		-d 					# запускает контейнер в background
		-it					# запускает контейнер в интерактивном режиме, но нужно указать ENTRYPOINT данного контейнера, например /bin/sh
		-p XXXX:YYYY		# проброс порта на host XXXX - порта на localhost YYYY - порт контейнера
		-e	ENV=value   	# задает переменную окружения для контейнера
		--name NAME			# задает имя контейнера, которое можно использовать вместо хэша
	--entrypoint top 	# данная команда будет выполнена при запуске контейнера
docker images 				# показывает список образов
docker rm <container>  		# удаляет контейнер
docker rm $(docker ps -aq -f status=exited) # удалит все остановленные контейнеры
docker rmi <image>			# удаляет образ
docker exec -it <container> <entrypoint> # заходит в работающий контейнер и запускает entrypoint
docker stop <container> 	# останавливает контейнер, переводит его в status=exited
docker system prune -a 		# удаляет все неиспользуемые контейнеры, директории, сети и образы
docker cp 			# копирует файлы CONTAINER_NAME:PATH DEST_PATH
```

## Exit коды

```bash
Exit 0 # отсутствует прикрепленный foreground процесс, контейнеру нечего делать
Exit 1 # произошла ошибка приложения, из за которой контейнер остановился (нужно смотреть внутрь контейнера)
Exit 137 # oom-killer убил контайнер из за нехватки памяти
Exit 64 #
```



## Dockerfile

Файл для создание собственного контейнера

Сборка происходит командой

```bash
docker build -f /path/to/ Dockerfile . # команда собирает образ по Dockerfile из указанной директории 
```



FROM  [Документация](https://docs.docker.com/engine/reference/builder/#from)

```dockerfile
FROM <image_name> #говорит, какой образ будет нужен
```
ENV  [Документация](https://docs.docker.com/engine/reference/builder/#environment-replacement) [Использование переменных окружения](https://docs.docker.com/engine/reference/builder/#env)

```dockerfile
ENV  foo \bar	# устанавливает переменную окружения, доспут можно получить - ${foo},
				# использование переменных окружения поддерживается следующими инструкциями: 
                # ADD
                # COPY
                # ENV
                # EXPOSE
                # FROM
                # LABEL
                # STOPSIGNAL
                # USER
                # VOLUME
                # WORKDIR
```
RUN [Документация](https://docs.docker.com/engine/reference/builder/#run)
```dockerfile
RUN <command> # какие комманды внутри образа нужно выполнить
```

ADD [Документация](https://docs.docker.com/engine/reference/builder/#add)

 ```dockerfile
ADD <from> <to> # копирует /скачивает документ 
 ```
```dockerfile
EXPOSE <port> # указывает какой порт входящих соединений
```

CMD [Документация](#https://docs.docker.com/engine/reference/builder/#cmd)

```DOckerfile
CMD ["/path/to/bin"] # в файле будет выполнена только последняя инструкция CMD
# есть несколько форматов: 
#	CMD ["exec", "param1", "param2"] // execform
#	CMD command param1 param2 // shellform
```

Аргументы docker run перезаписывают параметры CMD

CMD  в отличии от RUN не запускается в процессе docker build, однако указывает команду для образа.

ENTRYPOINT [Документация](#https://docs.docker.com/engine/reference/builder/#entrypoint)

```docker
ENTRYPOINT ["exec", "param1", "param2"]
```

ENTRYPOINT повторяет docker run --entrypoint , и может быть перезаписан этим параметром

## Docker-compose

### Основные директивы файла docker-compose.yml

[Общая документация](https://docs.docker.com/compose/compose-file/#service-configuration-reference)

Узнать какую спецификацию нужно использовать [тут](https://docs.docker.com/compose/compose-file/)

В скобках аналог команды в docker-cli

```yaml
version "3.7" # спецификация файла
services: # открытие блока перечисления сервисов
	db:	  # название сервиса
		image: nginx				# образ для сборки
		container_name: nginx_local # отображаемое имя контейнера (docker run --name nginx_local)
		ports:
		- XXXX:YYYY				# проброс порта на host XXXX - порта на localhost YYYY - порт контейнера (docker run -p XXXX:YYYY)
		volumes:
			- ./host/dir:/container/target/dir:ro # монтирует внешнюю папку/файл и устанавливает режим :ro - read-only :rw - read-write
```



### Основные команды docker-compose

```bash
docker-compose up --help # получить справку по ключам
docker-compose up 	# собрать и запустить сервис
				-d 	# запустить сервис в фоновом режиме
				--remove-orphans	# удаляет контейнеры, которые остались, но не описаны в docker-compose.yml
docker-compose ps # показать список сервисов запущенных через docker-compose
```

## Реестр (Docker Registry)

Реестр по умолчанию это [DockerHub](https://hub.docker.com)
Но можно сделать свой собственный реестр, в котором будут храниться наработки, которые не должны попасть в свободный доступ.

Такой реестр можно разместить на собственных серверах и брать образы оттуда.

### Создание

1. Создаем папку 
```bash
mkdir -p docker-registry/data
```
2. Создаем docker-compose.yaml
```yaml
//TODO
```
### Docker Tutorilal
```bash
docker run -d -p 80:80 docker/getting-started
```

Ключи запуска:
- `-d` - запустить в фоне (deattached mode)
- `-p 80:80` - соотнести 80 порт хост ситстемы с 80 портом контейнера
- `docker/getting-started` - название контейнера

#### Создание контейнера

Контейнер - изолированный процесс в системе.
Namespaces - 
Chroot - 
Cgroups - 

