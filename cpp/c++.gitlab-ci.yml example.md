
```yml
image: gcc

build:
	stage: build
	# предварительные ласки
	#before_script:
	#    - echo "Hello, $GITLAB_USER_LOGIN!"
	# основные ласки
	script:
		- g++ main.cpp -o main
		- make
	# завершающие ласки
	#after_script:
	#	- echo "all ok."
	artifactts:
		paths:
			- bin
#test:
#	stage: test
#	script:
#		- ./test.sh
#deploy:
#	stage: deploy
#	script: `echo "Define your deployment script!"`
#	environment: production
```

