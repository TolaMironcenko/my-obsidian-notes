```bash
sudo avrdude -v -patmega328p -c arduino -P /dev/ttyUSB0 -b 57600 -D -U flash:w:"arsh.hex":i
```

