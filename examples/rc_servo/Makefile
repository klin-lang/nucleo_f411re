CC := arm-none-eabi-gcc
CFLAGS := -mcpu=cortex-m4 -mthumb -ffreestanding -Os -ffunction-sections -fdata-sections
LDFLAGS := -T linker.ld -Wl,--gc-sections -nostdlib

KLIN ?= klin
ROOT := ../..

.PHONY: all emit elf clean deps

all: emit

deps:
	@case "$(KLIN)" in \
	  *.dart) dart run "$(KLIN)" get github/klin-lang/machine_stm32@v0.5.0 ;; \
	  *) "$(KLIN)" get github/klin-lang/machine_stm32@v0.5.0 ;; \
	esac

emit: out/rc_servo.c

out/rc_servo.c out/rc_servo.link: rc_servo.kl $(wildcard $(ROOT)/nucleo_f411re/*.kl)
	@mkdir -p out
	@case "$(KLIN)" in \
	  *.dart) dart run "$(KLIN)" --emit-c rc_servo.kl ;; \
	  *) "$(KLIN)" --emit-c rc_servo.kl ;; \
	esac

elf: rc_servo.elf

rc_servo.elf: linker.ld out/rc_servo.c out/rc_servo.link
	$(CC) $(CFLAGS) -I. $(LDFLAGS) $$(cat out/rc_servo.link) out/rc_servo.c -o $@

clean:
	rm -rf out rc_servo.elf
