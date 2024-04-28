# foo2zjs
HBPLv1  driver for CUPS

This is Dave Coffin's work from here:
http://cybercom.net/~dcoffin/hbpl/

this is a Linux driver I wrote for printers that use Host Based Printer Language version 1. You cannot use these printers in Linux without my driver. Known HBPLv1 printer models are:

Dell 1250c

Dell C1660w

Dell C1760nw

Epson AcuLaser C1700

Fuji-Xerox DocuPrint CP105b


There's also an HBPLv2, already supported in Linux, which compresses image data more efficiently. Less expensive printers tend to avoid version 2, either because it requires more processing power, or because it uses patented technology.

This list may be incomplete. If the self-test page says "HBPL" but the printer won't work with Linux, it's probably version 1. To use an HBPLv1 printer, either download this modified foo2zjs package or apply this patch to the offical foo2zjs release.

The essential commands are:

foo2hbpl1 to convert PBM, PGM, PPM, and PAM CMYK images to HBPLv1 format.

foo2hbpl1-wrapper to convert Postscript and PDF documents to HBPLv1 format.

hbpldecode to convert HBPL files, including those generated in other operating systems, to PGM or PPM images.

Images to print should be 5100x6600 pixels for US Letter paper or 4961x7016 pixels for A4 paper. Resolution is always 600x600 dpi. Stay at least 50 pixels (2mm) away from the edges to avoid clipping.


Printing HBPL files is easy -- either "cat" them to the printer device (/dev/usb/lp0 in my case), or use the "lpr" command.

## DW Changes (28-04-2024)

As read in [this raspberrypi forum post](https://forums.raspberrypi.com/viewtopic.php?t=73619&start=25) I implemented the change in [`foo2hbpl1-wrapper.in`](https://github.com/idarek/foo2zjs/commit/068f8d023b1934142a874ea2911fc284306b8171) file only to report right paper size and prevent error message (a seen in `tail /var/log/cups/error_log`) `foo2hbpl1-wrapper: Unimplemented paper code 1`.

Checked with **Epson AcuLaser C1700** with PPD file provided and **CUPS 2.3.3op2**.

### Installation method:

```
git clone https://github.com/idarek/foo2zjs.git
cd foo2zjs
sudo make clean
sudo make uninstall
make
sudo make instsll
sudo make cups
```

Then head to `https://raspberrypi.lan:631/admin` and add your printer.

At the step when selecting drivers, use from PPD.

Test your printer by printing Test Page at `https://raspberrypi.lan:631/printers/`
