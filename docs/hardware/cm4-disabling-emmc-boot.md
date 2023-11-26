# Disabling eMMC boot on the Raspberry Pi CM4

I recently embarked on a grand exploration of a fresh carrier board for my Raspberry Pi CM4s.

After some pondering, I settled the DeskPi Super 6C, a board capable of hosting six (yes six) CM4s and six (yes six) NVMe SSDs, all neatly packed inside a Mini-ITX form factor.

For this to work, I had to get it to boot from the NVME and to disable the eMMC storage for it to be of any real use.

Like many others, my initial attempt involved playing around with the RPiBOOT jumper, that handy switch most carrier boards feature (the one you tweak when flashing the eMMC). 

In spite of what the people of Google said, this ultimately failed. 

Then it hit me – the bootloader on a Pi 4 is as pliable as a Tories promise, and, better yet, configurable.

## Getting set up:

Let's set the stage with some prerequisites. 

```bash
sudo apt install git libusb-1.0-0-dev pkg-config
```

Grab the USB boot code with:

```bash
git clone --depth=1 https://github.com/raspberrypi/usbboot
```

Move into the directory with:

```bash
cd usbboot && make
```

Open the `boot.conf` file:

```bash
sudo nano recovery/boot.conf.
```

Now, in that `boot.conf` file, you'll spot a string called `BOOT_ORDER=0xf25641`.

This snazzy code dictates the boot priority order, or as the Raspberry Pi peeps call it, the Boot Order Codes.

The last digit decides which device takes priority during boot-up. The default is 1, the SD card or eMMC in our case.

But, hold your horses, we're going for NVMe. Edit the `boot.conf` to read `BOOT_ORDER=0xf25416` and save it.

## Moving on to the CM4 RPiBOOT mode. 

To dish out any EEPROM updates to the Raspberry Pi CM4 itself, you'll need to boot it into RPiBOOT mode.

The specifics vary from carrier board to carrier board, but usually, there's a jumper you can tweak before powering up the board. If all goes swimmingly, your CM4 won't boot up as usual but rather don its special mode attire.

Now navigate to the recovery directory with:

```bash
cd recovery && ./update-pieeprom.sh && ../rpiboot -d
```

At this juncture, you're practically done. Toss that RPiBOOT jumper away like yesterday's news, give your carrier board a reboot, and watch your CM4 waltz into the world of NVMe booting!

Oh, and in case you're wondering why I'm spreading this nugget of wisdom – well, I've had my fair share of bungled CM4 projects. Sold the lot on eBay for a pretty penny, though. So, this is the pinnacle of my CM4 prowess – take it or leave it!
