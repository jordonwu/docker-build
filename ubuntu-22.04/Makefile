all: build

build:
	#docker build -t jordonwu/docker-build:ubuntu-22.04 .
	# set proxy if need
	docker build --build-arg HTTP_PROXY="http://127.0.0.1:58591/" --build-arg http_proxy="http://127.0.0.1:58591/" --build-arg HTTPS_PROXY="http://127.0.0.1:58591/" --build-arg https_proxy="http://127.0.0.1:58591/"  --network host  -t jordonwu/docker-build:ubuntu-22.04 . 
run:
	docker run -it jordonwu/docker-build:ubuntu-22.04

deploy:
	docker push jordonwu/docker-build:ubuntu-22.04
