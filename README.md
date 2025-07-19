# Vornado HD293 Smart Fan Controller

Transform your Vornado HD293 fan into a smart, remotely controllable device with this ESPHome-based controller. This project replaces the manual speed selection switch with an ESP8266 microcontroller, enabling WiFi control through Home Assistant while maintaining offline functionality with a physical button.

## Features

- **3-Speed Control**: Full control over low, medium, and high fan speeds
- **Home Assistant Integration**: Native ESPHome integration with automatic discovery
- **Manual Control**: Physical button for offline speed cycling
- **Safety Interlocks**: Prevents multiple relays from being active simultaneously
- **Status Monitoring**: WiFi signal strength, uptime, and connection status
- **3D Printed Enclosure**: Custom enclosure design (available on Printables)
- **Over-the-Air Updates**: Easy firmware updates via WiFi

## Hardware Requirements

- **ESP8266 D1 Mini** (or compatible)
- **3x Relay Module** (5V, suitable for fan motor switching)
- **Push Button** (momentary, normally open)
- **Vornado HD293 Fan** (disassembled for modification)
- **Jumper wires and connectors**
- **3D Printed Enclosure** (STL files coming soon to Printables)

## Pin Configuration

| Component | ESP8266 Pin | Description |
|-----------|-------------|-------------|
| Push Button | GPIO4 | Manual speed control button |
| Low Speed Relay | GPIO14 | Controls fan low speed |
| Medium Speed Relay | GPIO13 | Controls fan medium speed |
| High Speed Relay | GPIO12 | Controls fan high speed |
| Status LED | GPIO2 | Built-in LED (connection status) |

## Installation

### Prerequisites

- [ESPHome](https://esphome.io/) installed
- [Home Assistant](https://www.home-assistant.io/) (optional but recommended)
- Basic electronics knowledge and tools

### Hardware Setup

⚠️ **Warning**: This modification involves working with mains voltage. Ensure power is disconnected before making any modifications. If you're not comfortable working with electrical components, please consult a qualified electrician.

1. **Disassemble the Vornado HD293 fan**
   - Remove the original speed selection switch
   - Identify the motor speed taps (typically 3 wires for different speeds)

2. **Install the ESP8266 and relay module**
   - Mount the ESP8266 D1 Mini in the 3D printed enclosure
   - Connect the relay module according to the pin configuration
   - Wire the relays to control the fan motor speed taps

3. **Add the manual control button**
   - Install the push button in an accessible location
   - Connect to GPIO4 with appropriate pull-up resistor (internal pull-up used in code)

### Software Setup

1. **Clone this repository**
   ```bash
   git clone https://github.com/undergroundpost/esphome-vornado-hd293.git
   cd vornado-hd293-smart-controller
   ```

2. **Create your secrets file**
   ```bash
   cp secrets.yaml.example secrets.yaml
   ```

3. **Edit the secrets file** with your WiFi credentials and encryption keys:
   ```yaml
   wifi_ssid: "YourWiFiNetwork"
   wifi_password: "YourWiFiPassword"
   # ... other secrets
   ```

4. **Flash the ESP8266**
   ```bash
   esphome run vornado-hd293.yaml
   ```

5. **Add to Home Assistant** (if using)
   - The device should be automatically discovered
   - Go to Settings > Devices & Services > ESPHome
   - Configure the new device

## Usage

### Manual Control
- **Single button press**: Cycles through fan speeds (Off → Low → Medium → High → Off)
- **Long press**: No action (reserved for future features)

### Home Assistant Control
- **Fan Entity**: Control speed and on/off state
- **Sensors**: Monitor WiFi signal strength, uptime, and IP address
- **Automation**: Create custom automations based on temperature, time, etc.

### API Control
The device exposes a REST API for direct control without Home Assistant:
```bash
# Turn on at medium speed
curl -X POST http://device-ip/fan/vornado_fan/turn_on -d '{"speed": 2}'

# Turn off
curl -X POST http://device-ip/fan/vornado_fan/turn_off
```

## Safety Features

- **Relay Interlocks**: Only one speed relay can be active at a time
- **300ms Wait Time**: Prevents relay chatter during speed changes
- **Input Debouncing**: 10ms debounce on button presses
- **Status LED**: Visual indication of device status and connectivity

## Troubleshooting

### Device Won't Connect to WiFi
1. Check your `secrets.yaml` file for correct credentials
2. Ensure the device is within WiFi range
3. Look for the fallback hotspot and connect to configure manually

### Fan Doesn't Respond
1. Check relay wiring and connections
2. Verify motor speed taps are correctly identified
3. Test relays individually using Home Assistant or logs

### Button Not Working
1. Check GPIO4 connection and pull-up configuration
2. Monitor logs for button press events
3. Verify button wiring (normally open, momentary)

### Viewing Logs
```bash
esphome logs vornado-hd293.yaml
```

## 3D Printed Enclosure

The custom enclosure STL files will be available on Printables soon. The enclosure features:
- Mounting points for ESP8266 D1 Mini
- Relay module compartment
- Button cutout and mounting
- Ventilation slots
- Cable management

**Print Settings:**
- Layer Height: 0.2mm
- Infill: 20%
- Supports: No
- Material: PLA or PETG

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

This project involves modifying electrical appliances and working with mains voltage. The authors are not responsible for any damage, injury, or loss resulting from the use of this information. Always ensure power is disconnected before making modifications, and consult a qualified electrician if you're unsure about any aspect of the installation.

## Acknowledgments

- [ESPHome](https://esphome.io/) for the excellent IoT framework
- [Home Assistant](https://www.home-assistant.io/) community for inspiration
- Vornado for making great fans worth upgrading

## Support

If you find this project helpful, consider:
- ⭐ Starring this repository
- 🐛 Reporting bugs or requesting features via Issues
- 📝 Contributing improvements via Pull Requests
- ☕ [Buying me a coffee](https://buymeacoffee.com/yourusername) (optional)

---

**Version**: 1.0  
**Last Updated**: July 2025
