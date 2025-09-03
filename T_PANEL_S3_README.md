# LILYGO T-Panel S3 Support

This project has been modified to work with the LILYGO T-Panel S3 display. The T-Panel S3 features a 480x480 RGB display with ESP32S3 microcontroller.

## Key Changes Made

### 1. PlatformIO Configuration (`platformio.ini`)
- Added new environment `FlightRadar24_T-Panel_S3` for T-Panel S3
- Updated display dimensions: `TFT_WIDTH=480`, `TFT_HEIGHT=480`
- Updated LVGL resolution: `LV_HOR_RES_MAX=480`, `LV_VER_RES_MAX=480`
- Configured for ESP32S3 with manufacturer-recommended settings:
  - Platform: espressif32 @6.5.0
  - Board: esp32s3_flash_16MB
  - Flash size: 16MB
  - Memory type: qio_qspi (external PSRAM)
  - Partitions: default_16MB.csv

### 2. Display Configuration
- **Display Driver**: ST7789 with parallel 8-bit interface
- **Resolution**: 480x480 pixels
- **Color Format**: RGB565 (16-bit)
- **Pin Configuration**:
  - TFT_DC: GPIO 7
  - TFT_RST: GPIO 5
  - TFT_WR: GPIO 8
  - TFT_RD: GPIO 9
  - TFT_D0-D7: GPIO 39-48
  - TFT_BL: GPIO 38

### 3. Layout Adjustments (`src/main.cpp`)
- **No Display Rotation**: Set to 0 degrees (square display)
- **Scaled Layout**: Adjusted all UI element positions for 480x480 display
- **Increased Margins**: Added proper spacing (20px margins)
- **Larger Text Areas**: Increased label widths for better readability
- **Better Spacing**: Improved vertical spacing between elements

### 4. Memory Configuration (`include/lv_conf.h`)
- **Increased LVGL Memory**: 64KB (from 48KB)
- **Larger Layer Buffers**: 32KB (from 24KB)
- **Display Buffer**: Increased to 20 lines (from 10)

### 5. UI Layout Structure
The flight information is now displayed in a more spacious layout:

```
┌─────────────────────────────────────────────────────────┐
│ Flight: ABC123    ABC-DEF    1/5                        │
├─────────────────────────────────────────────────────────┤
│ Altitude: 35,000ft    Vertical: +500ft/m    Speed: 450kts│
├─────────────────────────────────────────────────────────┤
│ Reg: N12345    Boeing 737-800                            │
├─────────────────────────────────────────────────────────┤
│ Lat: 40.7128°N, 74.0060°W              Heading: 270°   │
├─────────────────────────────────────────────────────────┤
│ American Airlines                                        │
├─────────────────────────────────────────────────────────┤
│ 🇺🇸 John F. Kennedy International Airport               │
│ 🇬🇧 London Heathrow Airport                             │
└─────────────────────────────────────────────────────────┘
```

## Building for T-Panel S3

To build and upload to the T-Panel S3:

```bash
# Build for T-Panel S3
pio run -e FlightRadar24_T-Panel_S3

# Upload to T-Panel S3
pio run -e FlightRadar24_T-Panel_S3 -t upload

# Monitor serial output
pio run -e FlightRadar24_T-Panel_S3 -t monitor
```

## Hardware Requirements

- LILYGO T-Panel S3 (with 16MB flash and external PSRAM)
- USB-C cable for programming and power
- Optional: GPS module (PA1010D) connected via I2C (pins 43, 44)

## Manufacturer Recommendations

The configuration follows the official LILYGO T-Panel S3 recommendations:
- **Platform**: espressif32 @6.5.0
- **Board**: esp32s3_flash_16MB
- **Memory**: External PSRAM enabled (qio_qspi)
- **Flash**: 16MB with default_16MB.csv partitions
- **USB**: CDC mode enabled for serial communication

## Button Configuration

- **Top Button**: GPIO 14 (used for configuration reset)
- **Bottom Button**: GPIO 0 (used for navigation)

## GPS Support

The T-Panel S3 configuration includes GPS support via I2C:
- **I2C Pins**: SDA=43, SCL=44
- **GPS Address**: 0x10 (PA1010D)
- **Update Interval**: 60 seconds

## Notes

- The T-Panel S3 has a much larger display than the original TTGO display, providing better readability
- All functionality remains the same - only the display layout has been optimized
- The square display format allows for better information density
- GPS functionality is preserved and will work with compatible I2C GPS modules

## Troubleshooting

1. **Display Not Working**: Check pin connections and ensure correct environment is selected
2. **Memory Issues**: If you see memory errors, try reducing font sizes or disabling some features
3. **GPS Not Detected**: Verify I2C connections and GPS module compatibility
4. **Upload Issues**: Ensure correct COM port is selected in platformio.ini

## Original vs T-Panel S3 Comparison

| Feature | Original TTGO | T-Panel S3 |
|---------|---------------|------------|
| Display | 135x240 | 480x480 |
| MCU | ESP32 | ESP32S3 |
| CPU Speed | 240MHz | 240MHz |
| Flash | 4MB | 16MB |
| RAM | 48KB LVGL | 64KB LVGL + PSRAM |
| Interface | SPI | Parallel 8-bit |
| Rotation | 90° | 0° |
| Layout | Compact | Spacious |
