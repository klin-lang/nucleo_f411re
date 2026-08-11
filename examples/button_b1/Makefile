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

emit: out/button_b1.c

out/button_b1.c out/button_b1.link: button_b1.kl $(wildcard $(ROOT)/nucleo_f411re/*.kl)
	@mkdir -p out
	@case "$(KLIN)" in \
	  *.dart) dart run "$(KLIN)" --emit-c button_b1.kl ;; \
	  *) "$(KLIN)" --emit-c button_b1.kl ;; \
	esac

elf: button_b1.elf

button_b1.elf: linker.ld out/button_b1.c out/button_b1.link
	$(CC) $(CFLAGS) -I. $(LDFLAGS) $$(cat out/button_b1.link) out/button_b1.c -o $@

clean:
	rm -rf out button_b1.elf
