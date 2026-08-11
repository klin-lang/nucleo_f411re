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

emit: out/i2c1.c

out/i2c1.c out/i2c1.link: i2c1.kl $(wildcard $(ROOT)/nucleo_f411re/*.kl)
	@mkdir -p out
	@case "$(KLIN)" in \
	  *.dart) dart run "$(KLIN)" --emit-c i2c1.kl ;; \
	  *) "$(KLIN)" --emit-c i2c1.kl ;; \
	esac

elf: i2c1.elf

i2c1.elf: linker.ld out/i2c1.c out/i2c1.link
	$(CC) $(CFLAGS) -I. $(LDFLAGS) $$(cat out/i2c1.link) out/i2c1.c -o $@

clean:
	rm -rf out i2c1.elf
