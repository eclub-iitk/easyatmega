This is only for linux users.

  Please use the following commands to install the avr-tools and gcc.
  
  ``` bash
  sudo apt install gcc gdb avr-libc avrdude binutils-avr gcc-avr srecord gdb-avr simulavr make
  ```
  ```bash
  avr-gcc -g -Os -S -mmcu=atmega328p -c main.c
  ```
  ```bash
  avr-gcc -g -mmcu=atmega328p -o main.elf main.o
  ```
  ```bash
  avr-objcopy -j .text -j .data -O ihex main.elf main.hex
  ```
  ```bash
  avrdude -c avrisp -p m328p -P /dev/ttyUSB0 -b 19200 -u -U flash:w:main.hex
  ```
