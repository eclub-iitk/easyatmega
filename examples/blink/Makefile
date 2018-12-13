MCU = atmega328p 
F_CPU = 1000000UL 
PROGRAMMER_TYPE = avrisp
PROGRAMMER_ARGS = -b $(BAUD) -P /dev/ttyUSB0
BAUD = 19200

CC = avr-gcc
CFLAGS = -g -Os -DF_CPU=$(F_CPU)
INCLUDES = -I .
OBJCOPY = avr-objcopy
TARGET = main
TARGET_ARCH = -mmcu=$(MCU)

LFUSE = 0x62
HFUSE = 0xd9
EFUSE = 0xff
WRITEFUSESTRING = -U lfuse:w:$(LFUSE):m -U hfuse:w:$(HFUSE):m -U efuse:w:$(EFUSE):m
READFUSESTRING = -U lfuse:r:low_fuse.hex:h -U hfuse:r:high_fuse.hex:h -U efuse:r:extend_fuse.hex:h

SOURCES = $(TARGET).c
OBJECTS = $(SOURCES:.c=.o)
HEADERS = $(SOURCES:.c=.h)
	
%.o: %.c $(HEADERS)
	$(CC) $(CFLAGS) -Os $(TARGET_ARCH) $(INCLUDES) -c $< -o $@

$(TARGET).elf: $(OBJECTS)
	$(CC) -g $(TARGET_ARCH) -o $@ $^

%.hex: %.elf
	$(OBJCOPY) -j .text -j .data -O ihex $< $@ 

all: $(TARGET).hex

flash: $(TARGET).hex
	avrdude -c $(PROGRAMMER_TYPE) -p $(MCU:atmega=m) $(PROGRAMMER_ARGS) -U flash:w:$<

write_fuse:
	avrdude -c $(PROGRAMMER_TYPE) -p $(MCU:atmega=m) $(PROGRAMMER_ARGS) $(WRITEFUSESTRING)

read_fuse:
	avrdude -c $(PROGRAMMER_TYPE) -p $(MCU:atmega=m) $(PROGRAMMER_ARGS) $(READFUSESTRING)

clean:
	rm -f *.o *.elf *.hex

debug: 
	@echo $(OBJECTS) $(HEADERS)
