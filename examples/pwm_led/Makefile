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

emit: out/pwm_led.c

out/pwm_led.c out/pwm_led.link: pwm_led.kl $(wildcard $(ROOT)/nucleo_f411re/*.kl)
	@mkdir -p out
	@case "$(KLIN)" in \
	  *.dart) dart run "$(KLIN)" --emit-c pwm_led.kl ;; \
	  *) "$(KLIN)" --emit-c pwm_led.kl ;; \
	esac

elf: pwm_led.elf

pwm_led.elf: linker.ld out/pwm_led.c out/pwm_led.link
	$(CC) $(CFLAGS) -I. $(LDFLAGS) $$(cat out/pwm_led.link) out/pwm_led.c -o $@

clean:
	rm -rf out pwm_led.elf
