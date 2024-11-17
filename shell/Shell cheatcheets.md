
## If ... elif ... else ... fi

```shell
if [ "$a" == "$b" ]; then
	sleep 1;
elif [ "$a" == "2" ]
	sleep 2;
else
	sleep 5;
fi
```

## Substring in string

```shell
if echo "hello world" | grep -q "hello"; then
	echo "yes"
else
	echo "no"
fi
```

## Switch case

```shell
case "$1" in
	hello)
		echo hello
		;;
	world)
		echo world
		;;
	*)
		echo error
		;;
esac
```

