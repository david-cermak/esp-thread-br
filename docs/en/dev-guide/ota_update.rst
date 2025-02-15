***************************
2.3. OTA Update Mechanism
***************************

The ESP Thread Border Router supports building the RCP image into the Border Router image. Upon boot the RCP image will be automatically downloaded to the RCP if the Border Router detects the RCP chip failure to boot.

Hardware Prerequisites
-----------------------

To perform OTA update, the following devices are required:

- An ESP Thread Border Router
- A Linux Host machine

In addition to the UART connection to the RCP chip, two extra GPIO pins are required to control the RESET and the BOOT pin of the RCP. For ESP32-H2 the BOOT pin is GPIO8.

Download and Update the Border Router Firmware
-----------------------------------------------

Create a HTTPS server providing the OTA image file, excute the follow command on your Linux Host machine:

.. code-block:: bash

    openssl s_server -WWW -key ca_key.pem -cert ca_cert.pem -port 8070

The private key and the certificate shall be accpetable for the Border Router. If they are self-signed, make sure to add the public key to the trusted key set of the Border Router.

Now the image can be downloaded on the Border Router:

.. code-block:: bash

    ota download https://${HOST_URL}:8070/br_ota_image

After downloading the Border Router will reboot and update itself with the new firmware. The RCP will also be updated if the firmware version changes.


The RCP Image Rollback Mechanism
---------------------------------

The RCP image is stored in a configurable SPIFFS partition under a configurable prefix. Prefix `ot_rcp` will be used in as an example.

Two folders `ot_rcp_0` and `ot_rcp_1` will be used to store the current and the backup RCP image. The RCP image will be stored in `ot_rcp_0` upon first boot. An extra key-value pair will be added in the nvs for storing the current RCP image index.

When applying the RCP update, the RCP image with the current index will be downloaded. After the reboot, the Border Router will detect the status of the RCP. If the RCP fails to boot, the backup image will be flashed to the RCP to revert the change.

When downloading the OTA image from the server, the current RCP image will be marked as the backup image and the previous backup image will be overridden. After the download completes, the current image index will be updated.


The OTA Image File Structure
-----------------------------

The OTA image is a single file containing the meta data of the RCP image, the RCP bootloader, the RCP partition table and the firmwares.

The image can be generated with script :example_file:`basic_thread_border_router/create_ota_image.py`. By default this file is called automatically during build and packs the `ot_rcp` example image into the Border Router firmware.

The RCP image header is defined as the following diagram:

    +---------------+----------------+---------------+
    |     0xff      |  Header size   |       0       |
    +---------------+----------------+---------------+
    |  File Type 0  |  File size     |  File offset  |
    +---------------+----------------+---------------+
    |  File Type 1  |  File size     |  File offset  |
    +---------------+----------------+---------------+

    ...


 The file type is defined as the following table:

 .. list-table::
   :widths: 25 50

   * - 0
     - RCP version
   * - 1
     - RCP flash arguments
   * - 2
     - RCP bootloader
   * - 3
     - RCP partition table
   * - 4
     - RCP firmware
   * - 5
     - Border Router firmware
