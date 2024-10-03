
Чтобы использовать общий кэш между всеми заданиями на нескольких раннерах и хостах, создайте раздел [runner.cache] в файле конфигурации config.toml.

```toml
[[runners]] 
	name = "Distributed-Cache Runner" 
	... 
	[runners.cache] 
		Type = "s3" 
		Path = "bucket/path/prefix" 
		Shared = true 
		[runners.cache.s3] 
			ServerAddress = "s3.amazonaws.com" 
			AccessKey = "<changeme>" 
			SecretKey = "<changeme>" 
			BucketName = "foobar" 
			BucketLocation = "us-east-1"
```

```toml
[runners.docker]   
	pull_policy = "if-not-present
```
\
