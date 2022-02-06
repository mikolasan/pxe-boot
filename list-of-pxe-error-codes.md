---
description: The following is a comprehensive list of PXE errors and their meaning.
---

# List of PXE error codes

{% hint style="info" %}
Copy paste from [https://knowledge.broadcom.com/external/article/152549/complete-list-of-pxe-error-codes-and-the.html](https://knowledge.broadcom.com/external/article/152549/complete-list-of-pxe-error-codes-and-the.html)
{% endhint %}

## Init/Boot/Loader Codes

* PXE-E00: Could not find enough free base memory.

PXE BaseCode and UNDI runtime modules are copied from FLASH or upper memory into the top of free base memory between 480K (78000h) and 640K (A0000h). This memory must be zero-filled by the system BIOS. If this memory is not zero-filled, the relocation code in the PXE ROMs will assume that this memory is being used by the system BIOS or other boot ROMs.

* PXE-E01: PCI Vendor and Device IDs do not match!

This message should never be seen in a production BIOS. When the system BIOS initializes a PCI option ROM, it is supposed to pass the PCI bus/device/function numbers in the AX register. If the PCI device defined in the AX register does not match the UNDI device, this error is displayed.

* PXE-E04: Error reading PCI configuration space.

This message is displayed if any of the PCI BIOS calls made to read the PCI configuration space return an error code. This should not happen with a production BIOS and properly operating hardware.

* PXE-E05: EEPROM checksum error.

This message is displayed if the NIC EEPROM contents have been corrupted. This can happen if the system is reset or powered down when the NIC EEPROM is being reprogrammed. If this message is displayed the PXE ROM will not boot.

* PXE-E06: Option ROM requires DDIM support.

This message should not be seen in a production BIOS. PCI option ROMs must always be installed as DDIM option ROMs (they must be installed into read/write upper memory).

* PXE-E07: PCI BIOS calls not supported.

This message should not be seen in a production BIOS. PCI BIOS must have PCI BIOS services.

* PXE-E08: Unexpected API error. API: xxxxh Status: xxxxh

This message is displayed if a PXE API returns a status code that is not expected by the runtime loader.

* PXE-E09: Unexpected UNDI loader error. Status: xxxxh

This message is displayed if the UNDI runtime loader returns an unexpected status code.

## &#x20;ARP Codes

* PXE-E11: ARP timeout.

The PXE ROM will retry the ARP request four times. If it does not get any valid ARP replies, this message is displayed. This error can be caused by a number of network and service configuration errors. The most common are:

1. Setting the DHCP Class Identifier (option 60) on the DHCP server and installing the proxyDHCP on a separate machine.
2. Using routers that do not respond to ARP requests.

## BIOS and BIS Codes

* PXE-E20: BIOS extended memory copy error. AH == nn

This message is displayed if the BIOS extended memory copy service returns an error. This should not happen on a production BIOS. The variable "nn" is the BIOS error code returned by the BIOS extended memory copy service (Int 15h, AH = 87h).

* PXE-E21: BIS integrity check failed.

This message is displayed if the BIS image in extended memory has been corrupted.

* PXE-E22: BIS image/credential validation failed.

The downloaded image and credential do not match the client key.

* PXE-E23: BIS initialization failed.

BIS could not be initialized. No more data is available.

* PXE-E24: BIS shutdown failed.

BIS could not be shutdown. No more data is available.

* PXE-E25: BIS get boot object authorization check flag failed.

Could not determine if BIS is enabled/disabled.

* PXE-E26: BIS free memory failed.

Could not release BIS allocated memory.

* PXE-E27: BIS get signature information failed.

Required BIS credential type information could not be determined.

* PXE-E28: BIS bad entry structure checksum.

BIS entry structure in the SM BIOS table is invalid.

## TFTP/MTFTP Codes

* PXE-E32: TFTP open timeout.

TFTP open request was not acknowledged. Verify that the TFTP service is running.

* PXE-E35: TFTP read timeout.

Next TFTP data packet was not received.

* PXE-E36: Error received from TFTP server.

A TFTP error packet was received from the TFTP server.

* PXE-E38: TFTP cannot open connection.

A hardware error occurred when trying to send the TFTP open packet out.

* PXE-E39: TFTP cannot read from connection.

A hardware error occurred when trying to send a TFTP acknowledge packet out.

* PXE-E3A: TFTP too many packages.

This message can mean one of two things: PXE-E3B: TFTP error - File not found.\
&#x20;The requested file was not found on the TFTP server.

* PXE-E3C: TFTP error ? Access violation.

The request file was found on the TFTP server. The TFTP service does not have enough access rights to open/read the file.

* PXE-E3F: TFTP packet size is invalid.

The TFTP packet received is larger than 1456 bytes.

Using TFTP, you are trying to download a file that is larger than the allocated buffer.

Using MTFTP, you started downloading a file as a slave client, and the file increased in size when you became the master client.

## BOOTP/DHCP Codes

* PXE-E51: No DHCP or proxyDHCP offers were received.

The client did not receive any valid DHCP, BOOTP, or proxyDHCP offers.

* PXE-E52: proxyDHCP offers were received. No DHCP offers were received.

The client did not receive any valid DHCP or BOOTP offers. The client did receive at least one valid proxyDHCP offer.

* PXE-E53: No boot filename received.

The client received at least one valid DHCP/BOOTP offer but does not have a boot filename to download.

* PXE-E55: proxyDHCP service did not reply to request on port 4011.

The client issued a proxyDHCP request to the DHCP server on port 4011 but did not receive a reply.

## UNDI Codes

* PXE-E60: Invalid UNDI API function number.

An API being used by the BaseCode is not implemented in the UNDI ROM.

* PXE-E61: Media test failed, check cable.

Most likely the cable is not plugged in or connected. Could be a bad cable, NIC, or connection.

* PXE-E63: Error while initializing the NIC.

An error occurred while trying to initialize the NIC hardware. Try another NIC.

* PXE-E64: Error while initializing the PHY.

An error occurred while trying to initialize the PHY hardware. Try another NIC.

* PXE-E65: Error while reading the configuration data

An error occurred while reading the NIC configuration data. Try another NIC.

* PXE-E66: Error while reading the initialization data.

An error occurred while reading the NIC initialization data. Try another NIC.

* PXE-E67: Invalid MAC address.

The MAC address stored in this NIC is invalid. Try another NIC.

* PXE-E68: Invalid EEPROM checksum.

The EEPROM checksum is invalid. The contents of the EEPROM have been corrupted. Try another NIC.

* PXE-E69: Error while setting interrupt.

The interrupt hardware could not be configured. Try another NIC.

## Bootstrap and Discovery Codes

* PXE-E74: Bad or missing PXE menu and/or prompt information.

PXE tags were detected, but the boot menu and/or boot prompt tags were not found/valid.

* PXE-E76: Bad or missing multicast discovery address.

Multicast discovery is enabled but the multicast discovery address tag is missing.

* PXE-E77: Bad or missing discovery server list.

Multicast and broadcast discovery are both disabled, or use server list is enabled, and the server list tag was not found/valid.

* PXE-E78: Could not locate boot server.

A valid boot server reply was not received by the client.

* PXE-E79: NBP is too big to fit in free base memory.

The NBP is larger than the amount of free base memory.

* PXE-E7A: Client could not locate a secure server.

This message is displayed when the client did not receive any security information from the boot server and BIS is enabled on the client.

* PXE-E7B: Missing MTFTP server IP address.

This message is displayed when the ROM did not receive any PXE discovery tags or proxyDHCP offers and the DHCP SIADDR field is set to 0.0.0.0.

## APITest.0 and DOSUNDI.0 Codes

* PXE-E81: !PXE structure is invalid.

The !PXE structure is missing or has been corrupted.

* PXE-E82: PXENV+ structure is invalid.

The PXENV+ structure is missing or has been corrupted.

* PXE-E83: Invalid DHCP option format.

PXE discovery tags in the cached packets are not valid.

* PXE-E84: Could not get pointer to original packet storage.

The PXE ROM being used did not return pointers to its local cached packet storage. The API test bootstrap program will not work with this boot ROM.

* PXE-E85: Not enough extended memory.

The size of the RAMdisk image is larger that the available extended memory.

* PXE-E86: ENV RAMdisk image corrupted.

The bootstrap program was expecting a DOS diskette image. The first 512 bytes of the downloaded image did not contain a DOS boot signature.

* PXE-E87: Could not find selected boot item.

The cached discovery reply packet did not contain a PXE boot-item tag (PXE option 71).

* PXE-E88: Could not locate boot server.

The bootserver did not reply to the RAMdisk image discovery request. The RAMdisk image is not on the bootserver, or the bootserver service is not running.

* PXE-E89: Could not download boot image.

The RAMdisk image could not be downloaded. The RAMdisk image is not on the bootserver, or the TFTP service on the bootserver is not running.

## Miscellaneous Codes

* PXE-EA0: Network boot canceled by keystroke.

User pressed ESC during DHCP/Discovery/TFTP.

## BaseCode/UNDI Loader Codes

* PXE-EC1: BaseCode ROM ID structure was not found.

UNDI boot module could not find the BaseCode ROM ID structure. If there is a BaseCode ROM image in the system, it has probably been corrupted.

* PXE-EC3: BaseCode ROM ID structure is invalid.

The BaseCode ROM ID structure is invalid. The BaseCode ROM image has probably been corrupted.

* PXE-EC4: UNDI ROM ID structure was not found.

The BaseCode loader module could not locate the UNDI ROM ID structure.

* PXE-EC5: UNDI ROM ID structure is invalid.

The UNDI ROM image has probably been corrupted.

* PXE-EC6: UNDI driver image is invalid.

The UNDI ROM image has probably been corrupted.

* PXE-EC8: !PXE structure was not found in UNDI driver code segment.

The UNDI ROM image has probably been corrupted or has not been initialized by the BIOS. This error is most often caused by one of three things:

1. A NIC image was programmed into a BIOS when a .LOM image should have been used.
2. The memory allocated by the POST Memory Manager ($PMM) during PXE option ROM initialization has been corrupted or erased before PXE option ROM boot.
3. The UNDI\_Loader structure was not properly initialized during option ROM initialization.

* PXE-EC9: PXENV+ structure was not found in UNDI driver code segment.

The UNDI ROM image has probably been corrupted or has not been initialized by the BIOS. This error is most often caused by one of three things:

1. A NIC image was programmed into a BIOS when a .LOM image should have been used.
2. The memory allocated by the POST Memory Manager ($PMM) during PXE option ROM initialization has been corrupted or erased before PXE option ROM boot.
3. The UNDI\_Loader structure was not properly initialized during option ROM initialization.
