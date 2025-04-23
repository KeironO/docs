# Adding a Zigbee Device to Home Assistant

## Introduction

Home Assistant makes it simple to connect Zigbee devices through the ZHA (Zigbee Home Automation) integration. This beginner-friendly guide walks you through the process of adding Zigbee devices to your Home Assistant setup.

## Prerequisites

Before you begin, ensure you have:

- **Home Assistant** installed and running on your device
- **Zigbee Coordinator** (such as a USB Zigbee adapter or a Zigbee-enabled hub)
- **Zigbee Device** that's compatible with ZHA (check the [Home Assistant compatibility list](https://www.home-assistant.io/integrations/zha/#known-working-zigbee-radio-modules))

## Step-by-Step Guide

### 1. Access Home Assistant Dashboard

Open your web browser and navigate to your Home Assistant dashboard.

### 2. Set Up the ZHA Integration

!!! note
    If you're using a system I've already configured, the ZHA integration is likely already installed, and you can skip directly to the pairing section.

#### Installing ZHA (if needed)

1. Navigate to **Settings** > **Devices & Services**
2. Click the **Add Integration** button in the bottom right corner
3. Search for "ZHA" and select the integration
4. Follow the on-screen instructions:
   - Select your Zigbee coordinator device
   - Specify the appropriate serial port (e.g., `/dev/ttyUSB0` or similar)
   - Configure any additional settings as prompted

#### Initialising Your Zigbee Network

If this is your first Zigbee device:

1. After completing the ZHA setup, your Zigbee network will be automatically initialised
2. You'll see a confirmation message once the network is ready

### 3. Pair Your Zigbee Device

1. Put your Zigbee device into pairing mode (refer to the device's manual for specific instructions)
   - Most devices enter pairing mode by holding a button for 5-10 seconds
   - Look for a rapidly blinking light indicating pairing mode
2. In Home Assistant, go to **Settings** > **Devices & Services**
3. Find and click on the **ZHA** integration card
4. Click **Add Device** in the bottom right corner
5. Home Assistant will search for your device (this typically takes 30-60 seconds)
6. Once discovered, the device will appear on screen with a proposed name
7. Confirm the device addition

### 4. Configure and Test

1. Once paired, navigate to the **Devices** tab
2. Find your newly added Zigbee device
3. Test basic functionality through the interface
4. Rename the device if desired by clicking on the device and then the gear icon

### 5. Troubleshooting

If you encounter issues during the pairing process:

- **Device not found**: Ensure your device is in pairing mode and within range of the coordinator
- **Connection timeout**: Move the device closer to your Zigbee coordinator
- **Device drops offline**: Consider adding [Zigbee repeaters](https://www.home-assistant.io/integrations/zha/#repeaters) to extend your network
- **Pairing failure**: Consult ZHA logs at **Settings** > **System** > **Logs** and filter for "zha"

#### Common Device-Specific Tips

- **Battery-powered sensors**: Make sure they have fresh batteries before pairing
- **Mains-powered devices**: Can serve as network repeaters, strengthening your Zigbee mesh
- **Stubborn devices**: Some may require a factory reset before pairing (refer to device manual)

## Advanced Configuration

Once your device is connected, you can:

- Create automations using your Zigbee device
- Add the device to dashboards
- Configure device-specific settings via **Settings** > **Devices & Services** > **ZHA** > [Your Device]

## Useful Resources

- [Official ZHA Documentation](https://www.home-assistant.io/integrations/zha/)
- [Home Assistant Community Forum](https://community.home-assistant.io/)
