#
# Makefile for AVR running FreeRTOS
#
# @date 12.05.2016
# @author Denise Ratasich
# 

#-------------------------------------------------------------------------
# project specific settings
# change these definitions for new projects
#-------------------------------------------------------------------------

# fill in a project name (name of the hexfile generated)
PROJNAME = freertos_test

# target architecture
MCU = atmega323

BINFORMAT = ihex

# programming parameters
PORT = /dev/ttyACM3
MC = ATmega323
DEVICE = stk500v2
BAUD = 115200

# application sources
APP_INC = include/
APP_SRC := $(wildcard src/*.c)

# FreeRTOS sources
RTOS_DIR = ../FreeRTOS_source/
RTOS_INC := $(addprefix $(RTOS_DIR),include/)
RTOS_SRC := $(addprefix $(RTOS_DIR),list.c queue.c tasks.c croutine.c) # timers.c event_groups.c

# port sources
PORT_DIR = ../FreeRTOS_source_ATmega323/
PORT_INC = $(PORT_DIR)
PORT_SRC := $(addprefix $(PORT_DIR),port.c heap_1.c)

# objects to build
OBJS := $(APP_SRC:.c=.o) $(RTOS_SRC:.c=.o) $(PORT_SRC:.c=.o)

# Flags
CFLAGS  = -mmcu=$(MCU) -Wall -O0 -I$(APP_INC) -I$(RTOS_INC) -I$(PORT_INC)
LDFLAGS	= -mmcu=$(MCU)
OCFLAGS	= -O $(BINFORMAT)
PRFLAGS = -c $(DEVICE) -b $(BAUD) -p $(MC) -P $(PORT) -e -v -v
#EEPFLAGS= --set-section-flags=.eeprom="alloc,load" --change-section-address .eeprom-0x810000

#-------------------------------------------------------------------------
# the targets
#-------------------------------------------------------------------------

all: bin/$(PROJNAME).hex bin/$(PROJNAME).elf

# example: avr-objcopy -O ihex demo.elf demo.hex
bin/%.hex: bin/%.elf
	avr-objcopy $(OCFLAGS) $< $@

bin/%.elf: $(OBJS)
	mkdir -p bin
	avr-gcc $(OBJS) $(LDFLAGS) -o $@

%.o: %.c
	avr-gcc $(CFLAGS) -c -o $@ $<


.PHONY: flash
# avrdude -c stk500v2 -p ATmega16 -P /dev/ttyUSB0 -e -U flash:w:demo.hex
flash: bin/$(PROJNAME).hex
	avrdude $(PRFLAGS) -U flash:w:bin/$(PROJNAME).hex


.PHONY: clean
clean:
	rm -f src/*~
	rm -f src/*.[do]
	rm -f $(SRC_FREERTOS)/*.[do]
	rm -f -r bin
