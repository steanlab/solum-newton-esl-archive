# Solum EL022F3WRA — nRF52811

## Идентификация

- Название: Solum Newton 2.2" ESL
- MCU: nRF52811-QFAA
- Архитектура: ARM Cortex-M4
- Размер main flash: 192 KiB
- Программатор: Raspberry Pi Pico / CMSIS-DAP v2
- Интерфейс: SWD

## Сохранённые регионы

- Main flash: `0x00000000`, 192 KiB
- UICR: `0x10001000`, 4 KiB
- FICR: `0x10000000`, 4 KiB
- SRAM: `0x20000000`, 24 KiB
- Code RAM: `0x00800000`, 24 KiB

## Проверка

Два mainflash-дампа были сравнены командой `cmp -s`; результат: `0`.
Контрольные суммы находятся в `metadata/SHA256SUMS.txt`.
