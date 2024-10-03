
Если том Docker не планируется использовать для кэширования, то при регистрации раннера установите опцию «монтирования связей» (bind mount). В этом случае уже не нужно задавать инструкцию cache в файле .gitlab-ci.yml.

```shell
gitlab-runner register \ 
	--name="Bind-Mount Runner" \ 
	--docker-volumes="/host/path:/container/path:rw" \ 
	...
```
