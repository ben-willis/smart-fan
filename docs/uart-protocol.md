# Fan UART Protocol

This document captures the reverse-engineered fan control protocol used by the original board.

## Serial Settings

- Baud rate: 334
- Payload: 5 bytes
- Byte order: fixed-length frame

## State Mapping

The frame encodes fan state as power, speed, and oscillation fields. The stock controller repeats frames continuously for the current state.

## Frame Format

`[START, POWER, SPEED, OSC, CHECKSUM]`

- START: expected `0x5A`
- POWER: power state bitfield
- SPEED: speed bitfield
- OSC: oscillation bitfield
- CHECKSUM: frame checksum used by the fan controller

## Known Command Frames

### Power Off

`{ 0x5A, 0x40, 0x80, 0x20, 0x9F }`

### Power On, Oscillation Off

- Low: `{ 0x5A, 0xC0, 0x80, 0x20, 0x1F }`
- Medium: `{ 0x5A, 0xC0, 0x40, 0x20, 0xEF }`
- High: `{ 0x5A, 0xC0, 0xC0, 0x20, 0x6F }`

### Power On, Oscillation On

- Low: `{ 0x5A, 0xC0, 0x80, 0xA0, 0xEF }`
- Medium: `{ 0x5A, 0xC0, 0x40, 0xA0, 0x6F }`
- High: `{ 0x5A, 0xC0, 0xC0, 0xA0, 0xAF }`

## Implementation Notes

- The original board repeats state frames continuously.
- Current implementation is command-driven from ESPHome fan state callbacks.
- If new modes are discovered, add them here first and then mirror in firmware.

## Open Questions

- Whether the stock display/input board supports a reliable readback channel
- Whether additional mode/timer commands exist beyond the currently mapped set
