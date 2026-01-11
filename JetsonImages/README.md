# JetsonImages - Hardware Platform Setup

This directory contains instructions and resources for setting up the hardware platform (NVIDIA Jetson Nano or Raspberry Pi 5) that runs the VEX AI vision processing system. The platform handles real-time AI inference, camera processing, and communication with the VEX V5 Brain.

## Supported Platforms

The VEX AI Competition system supports two hardware platforms:

- **NVIDIA Jetson Nano Developer Kit** - Uses CUDA/TensorRT for GPU-accelerated AI inference
- **Raspberry Pi 5** - Uses Google Coral Edge TPU (USB or M.2 PCI-E) for AI inference

Both platforms run the same vision processing code, but use different AI inference backends optimized for their respective hardware.

## Quick Setup: Using Pre-built Images (Recommended)

The fastest way to get started is to use our pre-built SD card images. These images include:
- Complete operating system (Ubuntu for Jetson, Raspberry Pi OS for Pi)
- All required drivers and dependencies pre-installed
- Intel RealSense SDK configured and ready
- AI inference libraries (TensorRT for Jetson, PyCoral for Raspberry Pi)
- VEX AI system configured to auto-start on boot
- Default login credentials (username and password: `password`)

### Prerequisites

Before flashing an image, ensure you have:

1. **[Balena Etcher](https://www.balena.io/etcher)** - Download, install, and launch this free tool for flashing SD card images
2. **SD Card** - A formatted SD card with at least **32GB** capacity (64GB recommended for better performance)
3. **SD Card Reader** - A way to connect the SD card to your computer

### Download Images

Download the appropriate image for your hardware platform:

- **[Jetson Nano Image](https://content.vexrobotics.com/V5AI/Images/Jetson/VAIC_25_26_JETSON_080625.img.gz)** (~4-6 GB compressed)
- **[Raspberry Pi 5 Image](https://content.vexrobotics.com/V5AI/Images/Jetson/VAIC_25_26_RPI_080625.img.gz)** (~4-6 GB compressed)

> [!WARNING]
> The Raspberry Pi image is built specifically for the **Raspberry Pi 5** with a **Google Coral Edge TPU** (USB or M.2 accelerator). It may not work correctly on other Raspberry Pi models.

### Installation Steps

1. **Flash the Image**:
   - Open Balena Etcher
   - Select the downloaded image file (.img.gz)
   - Select your SD card as the target
   - Click "Flash" and wait for the process to complete (10-30 minutes depending on SD card speed)

2. **Boot Your Device**:
   - Safely eject the SD card from your computer
   - Insert the SD card into your Jetson Nano or Raspberry Pi 5
   - Connect your Intel RealSense D435 camera via USB
   - Connect your VEX V5 Brain via USB (for serial communication)
   - Power on the device

3. **Verify System is Running**:
   - The system should automatically start the VEX AI vision processing on boot
   - The `vexai` systemd service runs the main Python application in the background
   - To check status: `sudo systemctl status vexai`
   - To manually start: `sudo systemctl start vexai`
   - To stop: `sudo systemctl stop vexai`

4. **Access Your Device**:
   - **Default credentials**: Username and password are both `password`
   - You can connect a display and keyboard, or SSH into the device
   - For headless setup, enable SSH before first boot or configure network settings

5. **Next Steps**:
   - Set up the [Web Dashboard](../JetsonWebDashboard/README.md) for monitoring and configuration
   - Configure your device as a Wi-Fi access point to access the dashboard wirelessly
   - Review the [JetsonExample](../JetsonExample/README.md) to understand the vision processing system
   - Upload code to your V5 Brain using [V5Example](../V5Example/ai_demo/README.md)

## Building from Source

If you prefer to build the system from scratch, need to customize the installation, or want to understand the setup process in detail, follow the platform-specific build instructions:

### When to Build from Source

Building from source is recommended if you:
- Want to customize the system configuration
- Need to use a different version of dependencies
- Want to learn about the underlying setup process
- Are troubleshooting issues with the pre-built image
- Want to modify or patch system components

### Build Instructions

Choose the guide for your platform:

- **[Jetson Nano Build Instructions](./BuildImageFromScratch.md)** - Complete step-by-step guide for setting up Jetson Nano from a fresh Ubuntu installation
  - Includes Intel RealSense SDK compilation with CUDA support
  - TensorRT installation and configuration
  - Python environment setup
  - System service configuration

- **[Raspberry Pi 5 Build Instructions](./PiInstructions.md)** - Complete step-by-step guide for setting up Raspberry Pi 5
  - Python 3.9 installation via pyenv (required for PyCoral compatibility)
  - Intel RealSense SDK compilation
  - Google Coral Edge TPU setup (USB or M.2 accelerator)
  - Gasket driver installation (for M.2 accelerators, includes patch file)
  - Wi-Fi hotspot configuration
  - System service configuration

### Additional Resources

- **`fix_gasket_driver.patch`** - Patch file for the Google Coral gasket driver (M.2 PCI-E accelerators only). This patch is automatically applied when following the Raspberry Pi build instructions for M.2 accelerators.

### Build Time Estimates

- **Jetson Nano**: 1-2 hours (including compilation time)
- **Raspberry Pi 5**: 45-90 minutes (Python compilation adds significant time)

Both builds require active internet connection and may take longer depending on your network speed and hardware performance.

## Troubleshooting

### Image Issues

- **Image won't flash**: Ensure you're using Balena Etcher (not other tools), and that your SD card is properly formatted
- **Device won't boot**: Verify the SD card is properly inserted and that you've flashed the correct image for your hardware
- **Slow performance**: Ensure you're using a Class 10 or better SD card (UHS-I recommended)

### System Service Issues

- **Service won't start**: Check logs with `sudo journalctl -u vexai -n 50`
- **Service crashes on boot**: Review the [JetsonExample README](../JetsonExample/README.md) for runtime requirements (camera, USB connections, etc.)

For platform-specific troubleshooting, see the detailed guides:
- Jetson-specific issues: See [BuildImageFromScratch.md](./BuildImageFromScratch.md#troubleshooting)
- Raspberry Pi-specific issues: See [PiInstructions.md](./PiInstructions.md)

## What's Next?

After setting up your hardware platform:

1. ✅ **Hardware Platform** (You are here) - JetsonImages
2. **Core Vision System** - [JetsonExample](../JetsonExample/README.md) - Understand how the AI processing works
3. **Web Dashboard** - [JetsonWebDashboard](../JetsonWebDashboard/README.md) - Set up monitoring and configuration interface
4. **V5 Robot Integration** - [V5Example](../V5Example/ai_demo/README.md) - Connect your robot to the vision system