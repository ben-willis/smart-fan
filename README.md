# Smart Fan Project

A WiFi-enabled smart fan controller built on ESP32-S3 with Home Assistant integration, using ESPHome for firmware management.

## Overview

This project transforms a traditional fan into a smart, connected device with the following capabilities:
- **WiFi Connectivity**: ESP32-S3 microcontroller for wireless control
- **Home Assistant Integration**: Full API integration for home automation
- **Physical Controls**: GPIO-based buttons for manual operation
- **IR Remote Support**: NEC protocol infrared remote control
- **Advanced Features**: 3-speed control, oscillation mode, preset modes (Sleep, Nature, Normal)
- **Custom PCB**: Professionally designed KiCad PCB with production-ready Gerber files

## Documentation

- [Docs Index](docs/README.md)
- [Project Story](docs/project-story.md)
- [UART Protocol](docs/uart-protocol.md)
- [Hardware Notes](docs/hardware-notes.md)

If you are new to this repository, start with [Project Story](docs/project-story.md), then use [UART Protocol](docs/uart-protocol.md) while reviewing firmware behavior.

## Hardware

### Microcontroller
- **Board**: ESP32-S3-DevKitC-1
- **Framework**: Arduino

### Physical Controls
- **On/Off Button**: GPIO11
- **Speed Control Button**: GPIO10 (cycles through 3 speeds)
- **Oscillation Toggle Button**: GPIO34
- **Mode Button**: GPIO9 (reserved for future use)
- **Timer Button**: GPIO8 (reserved for future use)

### IR Remote
- **Protocol**: NEC
- **Address**: 0xFE01
- **Supported Commands**:
  - On/Off: 0xFC03
  - Speed Cycle: 0xF807
  - Oscillation: 0xEC13

### Visual Indicators
- **Low Speed LED**: GPIO15
- **Medium Speed LED**: GPIO18
- **High Speed LED**: GPIO19
- **Normal Mode LED**: GPIO14
- **Rhythm Mode LED**: GPIO17
- **Sleep Mode LED**: GPIO20

### Communication
- **Fan UART**: GPIO43 (TX), GPIO44 (RX) @ 334 baud
- **Remote Receiver**: GPIO7 (inverted, NEC protocol via RMT channel 4)

## Features

### Fan Control
- **3-Speed Operation**: Low, Medium, High
- **Oscillation Mode**: On/Off toggle
- **Preset Modes**: 
  - Sleep mode
  - Nature mode
  - Normal mode
- **State Preservation**: Always powers off on restart

### Connectivity
- **WiFi**: Configurable SSID and password with fallback hotspot
- **API**: Home Assistant encrypted API communication
- **OTA Updates**: Over-the-air firmware updates via ESPHome

### Debugging
- **Serial Logging**: DEBUG level logging enabled
- **UART Debug**: Bidirectional UART debugging for fan communication
- **State Tracking**: Comprehensive logging of all state changes

## Project Structure

```
smart-fan/
├── README.md                  # Project documentation
├── secrets.yaml               # WiFi and API credentials (not in git)
│
├── firmware/                  # ESPHome firmware configuration
│   └── smart-fan.yml          # Main ESPHome configuration
│
├── hardware/                  # KiCad PCB design files
│   ├── smart-fan.kicad_sch    # Schematic
│   ├── smart-fan.kicad_pcb    # PCB layout
│   ├── smart-fan.kicad_pro    # Project file
│   ├── Gerber/                # Manufacturing files
│   │   ├── smart-fan-F_Cu.gbr
│   │   ├── smart-fan-B_Cu.gbr
│   │   ├── smart-fan-*.gbr    # Additional layers
│   │   ├── smart-fan-PTH.drl
│   │   └── smart-fan-NPTH.drl
│   └── production/            # Production data
│       ├── bom.csv            # Bill of materials
│       ├── designators.csv
│       ├── positions.csv      # Pick & place
│       └── netlist.ipc
│
├── docs/                      # Project docs, protocol notes, and hardware references
│
└── venv/                      # Python virtual environment
```

## Setup & Installation

### Prerequisites
- Python 3.8+
- ESPHome CLI or Web UI
- ESP32-S3-DevKitC-1 board
- USB-C cable for programming

### Configuration

1. **Create secrets.yaml** with the following variables:
   ```yaml
   api_encryption_key: "your_api_key_here"
   ota_password: "your_ota_password"
   wifi_ssid: "your_wifi_ssid"
   wifi_password: "your_wifi_password"
   ap_password: "fallback_hotspot_password"
   ```

2. **Create a virtual environment and install dependencies**:
   ```bash
   python -m venv venv
   source venv/Scripts/activate
   python -m pip install -r requirements.txt
   ```

3. **Build and Upload Firmware**:
   ```bash
   esphome run firmware/smart-fan.yml
   ```

4. **First Flash**:
   - Connect ESP32-S3 via USB-C
   - Select the COM port
   - Let ESPHome handle the flashing process

### WiFi & API Setup

- **Normal Operation**: Connects to specified WiFi SSID
- **Fallback Mode**: If WiFi connection fails, opens "Smart Fan Fallback Hotspot"
- **API Encryption**: Uses Home Assistant encrypted API for secure communication
- **OTA Updates**: After initial flash, updates can be done wirelessly

## Home Assistant Integration

Once configured, the Smart Fan will appear in Home Assistant with:
- **Fan entity**: Full control (on/off, speed, oscillation)
- **Preset modes**: Select Sleep, Nature, or Normal modes
- **Binary sensors**: Button press detection
- **Switches**: Individual control for lights and modes

Add to Home Assistant `configuration.yaml`:
```yaml
esphome:
  - firmware/smart-fan.yml
```

## Development

### Building PCB
- KiCad 8.0+ compatible
- Design files in `hardware/`
- Gerber files ready for ordering from fabricators
- Production data includes BOM, designators, and pick & place positions

### Serial Communication
The fan communicates via UART at 334 baud. Commands are 5-byte packets with checksums:
- Format: `[0x5a, SPEED_BITS, MODE_BITS, OSCILLATION_BITS, CHECKSUM]`
- See `docs/uart-protocol.md` for protocol details

### OTA Updates
After initial programming, update firmware wirelessly:
```bash
esphome run firmware/smart-fan.yml
```

## Debugging

### View Logs
```bash
esphome logs firmware/smart-fan.yml
```

### Monitor UART Communication
UART debugging is enabled with bidirectional logging. Connect to the serial monitor to see raw commands sent to the fan:
```bash
esphome logs firmware/smart-fan.yml
```

## Future Enhancements

- Mode button functionality for additional control modes
- Timer button integration for scheduled operation
- Display integration for status information
- Additional preset modes and customization
- Mobile app control via Home Assistant

## License

[Specify your license here]

## References

- [ESPHome Documentation](https://esphome.io/)
- [Home Assistant](https://www.home-assistant.io/)
- [ESP32-S3 DevKitC-1](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/hw-reference/esp32s3/user-guide-devkitc-1.html)
- [KiCad Documentation](https://docs.kicad.org/)
