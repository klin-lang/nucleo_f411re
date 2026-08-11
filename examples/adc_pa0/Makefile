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

emit: out/adc_pa0.c

out/adc_pa0.c out/adc_pa0.link: adc_pa0.kl $(wildcard $(ROOT)/nucleo_f411re/*.kl)
	@mkdir -p out
	@case "$(KLIN)" in \
	  *.dart) dart run "$(KLIN)" --emit-c adc_pa0.kl ;; \
	  *) "$(KLIN)" --emit-c adc_pa0.kl ;; \
	esac

elf: adc_pa0.elf

adc_pa0.elf: linker.ld out/adc_pa0.c out/adc_pa0.link
	$(CC) $(CFLAGS) -I. $(LDFLAGS) $$(cat out/adc_pa0.link) out/adc_pa0.c -o $@

clean:
	rm -rf out adc_pa0.elf
