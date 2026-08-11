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

emit: out/spi2.c

out/spi2.c out/spi2.link: spi2.kl $(wildcard $(ROOT)/nucleo_f411re/*.kl)
	@mkdir -p out
	@case "$(KLIN)" in \
	  *.dart) dart run "$(KLIN)" --emit-c spi2.kl ;; \
	  *) "$(KLIN)" --emit-c spi2.kl ;; \
	esac

elf: spi2.elf

spi2.elf: linker.ld out/spi2.c out/spi2.link
	$(CC) $(CFLAGS) -I. $(LDFLAGS) $$(cat out/spi2.link) out/spi2.c -o $@

clean:
	rm -rf out spi2.elf
