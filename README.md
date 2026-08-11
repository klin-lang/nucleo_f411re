# nucleo_f411re

Board pack for [STM32 Nucleo-F411RE](https://www.st.com/en/evaluation-tools/nucleo-f411re.html)
in [Klin](https://github.com/klin-lang/klin).

Not a CubeMX HAL port. No GC, no hidden heap, no hidden clocks.

Chip API: [`machine_stm32`](https://github.com/klin-lang/machine_stm32) `@v0.5.0`.
This package adds **Nucleo pin helpers**, a CubeMX-style **`.ioc` pinout seed**, and
board-shaped **examples** (LED, VCP UART, PWM, ADC, button, I2C, SPI, RC).

Klin issues: [075](https://github.com/klin-lang/klin/blob/main/issues/075-board-pack-init-host.md)
(board pack / init), [074](https://github.com/klin-lang/klin/blob/main/issues/074-board-ioc-klin-mod.md)
(`$board` / `.ioc`), [061](https://github.com/klin-lang/klin/blob/main/issues/061-micropython-machine-api.md).

## Status (`@v0.1.2`)

| Piece | Status |
|---|---|
| Pin helpers (`ld2_*`, `b1_*`, `vcp_*`, I2C1, SPI2, A0) | ✅ |
| `nucleo_f411re.ioc` seed for `$board` | ✅ |
| Examples: blink, uart_vcp, pwm_led, adc_pa0, button_b1, i2c1, spi2, rc_servo | ✅ |
| Freestanding `startup.s` / `linker.ld` per example | ✅ |

`version()` → `1`.

## Pins

| Signal | GPIO | Notes |
|---|---|---|
| LD2 (green LED) | PA5 | TIM2_CH1 AF1 for PWM/RC |
| B1 (user button) | PC13 | active low |
| ST-Link VCP TX/RX | PA2 / PA3 | USART2 AF7 |
| I2C1 SCL/SDA | PB8 / PB9 | AF4 (Arduino D15/D14) |
| SPI2 SCK/MISO/MOSI | PB13 / PB14 / PB15 | AF5 |
| A0 | PA0 | ADC1_IN0 |

Clock assumption in demos: **HSI 16 MHz** at reset (`hsi_hz()`).

## Usage

```klin
import "github/klin-lang/machine_stm32" machine
import "github/klin-lang/nucleo_f411re" board

fn main() {
  let led = machine.pin_out(cast(machine.Port, board.ld2_port()), board.ld2_pin())
  led.toggle()
}
```

Pinout via Klin `$board` (local copy preferred):

```klin
$board("board/nucleo_f411re.ioc")
// or after klin get: github/klin-lang/nucleo_f411re/nucleo_f411re.ioc
```

```sh
klin get github/klin-lang/nucleo_f411re@v0.1.2
klin get github/klin-lang/machine_stm32@v0.5.0
cd examples/blink && make KLIN=/path/to/bin/klin.dart deps emit elf
```

Needs `arm-none-eabi-gcc`. Flash with OpenOCD / probe-rs / STM32CubeProgrammer.

## Scaffold

```sh
klin init nucleo-f411 my_blink
# ships local board/*.ioc + startup; then optionally:
#   require github/klin-lang/nucleo_f411re v0.1.2
```

## License

MIT — see [LICENSE](LICENSE).
