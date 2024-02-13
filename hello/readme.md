### RGBDS

```
https://rgbds.gbdev.io/install
```


```bash
rgbasm -L -o hello.o main.asm
rgblink -o hello.gb hello.o
rgbfix -v -p 0xFF hello.gb
```

```bash
mgba-qt hello.gb
```
