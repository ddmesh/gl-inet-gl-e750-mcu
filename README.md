# gl-inet-gl-e750-mcu
The GL-Inet / GL-E750 LTE Wifi Router contains a display that is controlled by a seaparate MCU.

When using the "update-mcu" utility it is very likely that you brick this MCU. This utility is
badly programmed without any image checks.
https://github.com/gl-inet/GL-E750-MCU-instruction contains the description and a firmware for
this MCU. 
Copyright of this firmware and the tools to flash the MCU belong to the owner of the repo or
tool vendor.

This repo here just gives more information and provided the tools to repair the display.

# MCU
GL-e750 uses NUC029ZAN as the MCU. This contains two flash regions, "LDROM" and "APROM".\
A configuration register defines from which region the device boots. Each application may
access and flash the other region, which allows to upgrade the "loader" or the "application"
inplace.

When GL-e750 boots also the mcu boots its application. This application is responsible for
getting the "json" structure that describes the information that schould be displayed. The
application on the MCU then defines how this information is displayed with predefined text
and symbols.

The firmware on the GL-Inet (openwrt) provides this json to the MCU via internal UART. 
When the upgrade-mcu tool is used, the MCU application reconfigures the config register so that
the application from LDROM will be loaded. This loader application then expectes the MCU firmware
from GL-INET (openwrt) via UART. But there is absolutely no check for correct data. 
The Loader is stupid and just takes any information from the UART. If you Ctrl-C the update-mcu
tool and try to restart, it will receive the "UPDATE" command (simple text) and use this as
firmare. This is one why to brick the display.

