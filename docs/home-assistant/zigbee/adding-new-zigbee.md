# Adding a Zigbee Device

## Introduction

Home Assistant makes it simple to connect Zigbee devices through the ZHA (Zigbee Home Automation) integration. This beginner-friendly guide walks you through the steps of adding a Zigbee device to Home Assistant.

## Prerequisites

1. **Home Assistant Installed:** Ensure you have Home Assistant set up on your device.

2. **Zigbee Coordinator:** You need a Zigbee coordinator, like a USB Zigbee adapter or a Zigbee-enabled hub.

3. **Zigbee Device:** Choose a Zigbee device compatible with ZHA. Check Home Assistant's documentation for supported devices.

## Step-by-Step Guide

### 1. Access Home Assistant Dashboard

Open your web browser and go to your Home Assistant dashboard.

### 2. Install ZHA Integration

!!! note
    If you're using something I've set up, you're unlikely to actually want to do this as it has already been set up!

- Navigate to "Supervisor" or "Add-ons" in the sidebar.
- Click "Add-on Store."
- Search for "ZHA" and install the ZHA add-on.
- Start the add-on after installation.


Now plug in your USB Zigbee adapter or follow instructions to integrate your Zigbee-enabled hub.

Once that's done...

- Go to "Configuration" and select "Integrations" in the sidebar.
- Click "+" to add a new integration.
- Choose "ZHA" and follow on-screen instructions to set it up.

Now it's time to start our new Zigbee network.

- Under ZHA integration, click "Options."
- Click "Start Zigbee Network" to initiate it.

### 3. Pair the Zigbee Device

- Put your Zigbee device in pairing mode.
- In ZHA integration, click "Add Device."

### 4. Test and Enjoy

- Test your Zigbee device in Home Assistant.
- Customise settings or automations as needed.

## Troubleshooting Tips

- If the device doesn't pair, ensure it's in pairing mode and try again.
- Check ZHA logs for any error messages.
- Ensure the Zigbee coordinator is within range.

Congratulations! You've successfully added a Zigbee device to Home Assistant using ZHA. Enjoy your smart home!
