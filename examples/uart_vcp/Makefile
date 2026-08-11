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

emit: out/uart_vcp.c

out/uart_vcp.c out/uart_vcp.link: uart_vcp.kl $(wildcard $(ROOT)/nucleo_f411re/*.kl)
	@mkdir -p out
	@case "$(KLIN)" in \
	  *.dart) dart run "$(KLIN)" --emit-c uart_vcp.kl ;; \
	  *) "$(KLIN)" --emit-c uart_vcp.kl ;; \
	esac

elf: uart_vcp.elf

uart_vcp.elf: linker.ld out/uart_vcp.c out/uart_vcp.link
	$(CC) $(CFLAGS) -I. $(LDFLAGS) $$(cat out/uart_vcp.link) out/uart_vcp.c -o $@

clean:
	rm -rf out uart_vcp.elf
