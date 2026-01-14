# USB Bridge CAN Firmware Flashing

## Connect to Host Computer

Core Connection Limitation

The Fly-DP5 mainboard **can only communicate with the host computer for Klipper via the Type-C USB interface**.

![](https://docs.3dmellow.com/en/assets/images/host-usb-85885ec96a7691cb20d8a11607ef45c7.webp)

## Firmware Compilation Notes and Operation Guide

### Important Notes

📌 Important Prerequisites

1.  **Network Connection**: Ensure the host machine (Raspberry Pi, etc.) is connected to the network.
2.  **Access Method**: You must log in to the host machine via **SSH over the network**. Using serial port tools is prohibited.
3.  **User Permissions**: Use the correct user account for operations based on the host machine's system type.
4.  **Input Method**: Ensure the keyboard input method is set to **Half-width mode (English mode)**.

---

### SSH Login and User Switching

#### SSH Login to Host Machine

Use an SSH tool (such as MobaXterm, PuTTY, etc.) to log in to the host machine over the network:

#### Switch User Based on System Type

*   Standard Host Machine (Armbian)
*   FLY Host Machine

**Applicable Systems**:

*   Official Raspberry Pi OS
*   Other systems with Klipper installed

**User Permission Requirements**:

*   **Do not use the** `**root**` **user** for any operations.
*   You must switch to a **standard user** for operations.

**Switch Command**:

Other systems (replace `<username>` with your username)

```
su <username>
```

💡 Tip

Standard users typically have the necessary compilation permissions. Using the root user may cause permission issues.

---

### Firmware Compilation Instructions

#### 1\. Keyboard Operation Guide

*   In the Klipper firmware configuration page, you can only use the following shortcut keys for operation.
*   You cannot use the mouse directly!

| Key | Function Description |
| --- | --- |
| **↑ ↓ Arrow Keys** | Move the cursor up/down to select menu items. |
| **Enter** or **Space** | Confirm selection/check menu items or enter submenus. |
| **ESC** | Return to the previous menu level. |
| **Q** | Exit the Klipper firmware configuration page. |
| **Y** | Press Y to save the configuration if prompted when exiting. |

#### Show Hidden Options

⚠️ Show Hidden Options

If there are few options on the configuration page, first check:

```
[ ] Enable extra low-level configuration options
```

This option is used to display some hidden configuration options.

## Start Firmware Compilation

The following describes how to compile the firmware:

After connecting via SSH, enter the following command and press Enter:

```
cd ~/klipper && rm -rf ~/klipper/.config && rm -rf ~/klipper/out && make menuconfig
```

Among these, `rm -rf ~/klipper/.config && rm -rf ~/klipper/out` deletes previous compilation data and firmware.

`make menuconfig` is for compiling the firmware. After execution, the following interface should appear:

![](https://docs.3dmellow.com/en/assets/images/make1-378664726238f9ae2aef7a639a4ace5a.webp)

Select **Enable extra low-level configuration options** and press Enter.

![](https://docs.3dmellow.com/en/assets/images/make2-8ab51c82a046868b4f9058605be96476.webp)

Enter the menu **Micro-controller Architecture** and select **STMicroelectronics STM32**, then press Enter.

![](https://docs.3dmellow.com/en/assets/images/make3-199f6109c99dc156cc2c5c509b8c4c44.webp)

Enter the menu **Processor model**, select **STM32F072**, and press Enter.

![](https://docs.3dmellow.com/en/assets/images/072-5229a9cfafe4811b22468b602d21a006.webp)

Select **Bootloader offset**, choose: **8KiB bootloader**.

![](https://docs.3dmellow.com/en/assets/images/8kb-7be234a393219798ad0a841a56d5d2c6.webp)

Select **Communication interface**, choose: **USB to CAN bus bridge (USB on PA11/PA12)** and press Enter.

After selecting **USB to CAN bus bridge (USB on PA11/PA12)** and pressing Enter, the default parameter **CAN bus interface (CAN bus (on PB8/PB9))--->** does not need to be changed.

![](https://docs.3dmellow.com/en/assets/images/utoc-11d5070770636478e332b75fa9a67f00.webp)

**Please compare with the image below to verify, ensure it matches before proceeding with compilation.**

![](https://docs.3dmellow.com/en/assets/images/utoc2-2c00402be2a12275e725095f2f95a74b.webp)

*   Press the `Q` key, and **Save configuration** will appear. Then press the `Y` key.

![](data:image/webp;base64,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)

*   The configuration should now be saved, and you should have exited to the command line interface.
*   Enter the following command to start compilation. It will take some time.

```
make -j4
```

![](https://docs.3dmellow.com/en/assets/images/make-f49ff23c8f1a680a7e1835c9dcd759ba.webp)

*   Finally, if the output shows the following content, the compilation is successful.
*   Due to Klipper version differences, as long as `out/klipper.bin` appears, it indicates success.

```
Linking out/klipper.elf
Creating bin file out/klipper.bin
```

## Enter Flashing Mode

*   After connecting the mainboard to the host computer using a Type-C data cable, **double-click the RST button**. At this point, the LED on the mainboard will flash.

Warning

*   If the LED does not flash, please re-flash the katapult firmware:
*   If the LED does not flash, after flashing the katapult firmware, proceed to the next step.

![](https://docs.3dmellow.com/en/assets/images/boot-30f525191c64025162043a3bf1de6fb6.webp)

*   Please ensure that the host machine can connect to the network normally and ensure that the data cable connecting the lower-level machine to the host machine has data transmission function.
*   After SSH connecting to the host machine, input `lsusb` and press Enter. If the information circled in the following picture appears, you can proceed to the next step.
    *   If there is no feedback at all, this is a system problem of the host machine, and we are unable to help. You need to replace with a confirmed normal system or replace the host machine.
    *   If the prompt says there is no `lsusb` command, you can execute the following command to install it:

```
sudo apt-get install usbutils
```

![](https://docs.3dmellow.com/en/assets/images/6177-1c73cdd0d9f211a723e9cfe826ffc5c4.webp)

*   `1d50:6177` belongs to the device you will use this time.
    *   Some host machines may not fully display or fail to display due to system problems.
    *   If the `lsusb` command can display the device but does not show `1d50:6177`, please try replacing the data cable and connecting the mainboard to another USB port on the host machine.
*   If you have executed the above steps before and successfully burned the Klipper firmware, and the mainboard is running normally, but you just want to update the Klipper firmware, please directly check the `Firmware Update` section in the right menu bar of this page.

Notice

You must query the `1d50:6177` device before proceeding to the next step.

### Flash Firmware

## Start Flashing

📌 Prerequisites

*   **Internet connection** is required when installing the flashing plugin. Please ensure your host computer is connected to the internet.
*   If the flashing plugin is already installed, there is no need to reinstall it.

### Install Flashing Dependencies

⚠️ Important

If you are using a **non-Fly official host computer**, you must execute the following command to install the firmware flashing dependency package!

*   GitHub Repository
*   Domestic Mirror

```
cd && git clone https://github.com/Arksine/katapult.git
```

![](https://docs.3dmellow.com/en/assets/images/get_katapult-9d6d1f3ac4c2ca7715dc31868e3636fd.webp)

---

### Get Board ID

Execute the following command to search for the device ID. Normally, an ID similar to the one shown in the image below should be displayed (**Note: The ID is different for each board**):

```
ls /dev/serial/by-id/*
```

![](https://docs.3dmellow.com/en/assets/images/by-id-a50400eb0225d4504b43da8081144a8e.webp)

---

### Flash Firmware

⚠️ Preparation

*   Ensure the firmware file has been compiled.
*   Replace `<Your Board ID>` in the command below with the actual ID queried in the previous step.
*   Standard Host
*   Fly Host Flashing Tutorial

```
python ~/katapult/scripts/flashtool.py -d /dev/serial/by-id/<Your Board ID>
```

**Flashing Process Reference:**

![](https://docs.3dmellow.com/en/assets/images/kat-id-37d60cd8410f4c47510f5bb8398b9fac.webp)

**Successful Flashing Reference:**

![](https://docs.3dmellow.com/en/assets/images/flash-12a1a721fe3b9c996ac6999aa0d935a2.webp)

---

## Firmware Update

*   USB Firmware Update
*   Bridged CAN Firmware Update

### USB Firmware Update Steps

1.  **Query Board ID**

```
ls /dev/serial/by-id/*
```

Identification Key

In the image below, `/dev/serial/by-id/usb-katapult_rp2040_E662549553642032-if00` is the board ID.

![](data:image/webp;base64,UklGRrggAABXRUJQVlA4IKwgAAAQbQCdASqQAjQAPm00lEckIyIhKvMpyIANiU3cLhAbrgqsv0F5mIcez8grTB8Y82+kL/Q+or/L+oB59fQT5gPOD9DH9i9QD+Z/6b1j/+77B39v/7H//9wD9zPWi/+nsY/4f/z+m16gH/89r/+Af//iGf7f2e/4f+0/tX55+I7zP+u/t//aPYZ/rPCx51+q/47+5ep/8V+tf4P+s/3f/s+sf+A8Gfe1/gf2j8S/kF/H/5F/mv7Hv2eef3r/g+oF6p/Lv9z/dv8p/2fNt/jfRH8v/qX/T/svwAfxz+jf7/7jviD/W+D79K/vf/G/2vwA/yj+sf6v+2/5f9ifpO/hf+7/k/9N+5/sv/Lv73/1f8r/o/kF/kX9E/3n+C/zP7Vf/////eD///b/+3P/09x/9gP/6ZbyhChoQ0+BQS19N7N1GXK9DFnBfZucQOg1nbc1tO3w07Z8xTJhi/14YfP5WHpdn5nVYpV9CfrGCHdEH6nMy1XQ2hfIuVqi9v3lDAXSWuyHa9/0hr5dazKtNDtZ56EjlwLuldZnZJ64AG2KJqLh0btwAVEbyum3D5mO6LsbMu2+T6G3QFoPPF3K6cIznJYDo75M3v4Hkp420ZfRSRks+ba13lNMJZFHE9cqcQDQhZBXZtMDu2fNX8Ma5xXbqr4Dd2r5R4lBGCxts4xCKU0MYInUNOgad0zWZBi3kgduUrtoiu1T6P1EPPUqovsJqIKV8UeKqEMCYq7GtevigGse42WParlocxgLP3Mfpcwj9ruVrPAkJF73F29/qGKAQZvl+lYRsisa2DtAgyF9P7W8+rv+RAJkVohGmNmCziSRr/+6Hohp7NjzR53pKwkyqj2R3H1nisG0Vu6Y3IaqC7tH7C/wRe0Boz+0FylEbMdPrUwY9aQ19hf7TlEbD/nBQwiOg/eaUka9+Xn1E2XERTf/kD9biEMDLf/9dTNexB6hQ3C1aQ0zhZkaJT3+3xg20+1jp4t3IW2Ewyie2b8Rp15x0SCRbkaIZRAy5fZ+gvScSKReATl4NHglegz6mRXjQlgP3U5O9izdoZDc+Qo98p0mpW0FFsLUNhsQSDDzk1J8vimIkiAkJenySmbu1n/ZeWRgMMjXEpJb9qp3g6dkksXRObp4MUtHF/czBGd0FVnono50cyd8KM/+TGJUEqQ2VIKkAAD9O3x0y+aJQIz+D2wBBO6IZhMznEWhCbQ1F+BvsGnZJtyUu4ygxZ0WQ5lSPtKQR3oazp8yDETRAGEi1xj2JZwiWPClPRf1rt9k9uFCghNyOfoJcSK7AaJH2O18I0q7iFwGS0LO+8AEXOVdVUkTZkjeI4PzF1PxtB+kg/9Qop7rLcDGkKUSANUVyCTg2mGNvdHQ96CAO+SbgOpLiueG3UDNWa/DNZLFpzJ0dqnWU1ME6Px+H+QEQXxQdMyIr5+s8/WPgojmy3+T9daV0wImgRfceXIqFXEmEyW+Ihh3g4xNNhCluYyZERu0RM49/bo1/nOXEC/c6NUPN0Z39WW9+wRPvNGp0ijOdt27JFHhgxvQJnsNGcXK2ConV3Ii0Q1/iUT2gZ9Kq+lYLh/9MQW12WOF2FqHzPnJwIwoD1Sn1EsZnyZPD7DqNatl9Lmzr62ADKnMwNqW3yIi5S8aT9w++ZLM/48RjSc2J0XeL5hs85jFkkKruOLViOnr1Lg1LQnda/7+/IotBoKKVM2Y1cKxjsT+0rxVYZGO2pcFVsbj1EQq8POk02F6SMKrMUOVm2fzVGYAB2F6kRsWFMiOzMSHmY/PjEc7stxcTqg4rrpSdNqGm6ad9N17Z+1uuNAGGMqayA9vXZnGGr5fUMrvA9VvDqAYTqShQOCXsSG0vFpdce9dbNcv5ZRc1kguJn9LCfqfQ3Dq+1CsrvJAoQx+wh39zd9K1kHoiUquPE7HoAA/CDc5/t/IcbQQEOQYSomn3gv1csziQ6nmscLxkvripW8yUbI0uutlJB6FnPUfVVqRe7dKuonUpGwSHwh1PQzbiIIX0GvfRPy15v7hJ1L2e/t4CwcmaZ0J9+KrgSNZc6/xtzl+Pa/bU2oc8nEBXg4X1sgIHbXGbakn7ubhD6My4X7OuQLqzHE8lzsh48rnAZJD9bZeW0+E4S67F2EnLdvE/IRVhCfiUXbTErp1dqTkB9qYThJBbrSQAvtOu8rpmc9zC3dD4CHbtjiqAEIx4Ityr4QL4ec6K8xT75MlacpJgbCTrYZ0xhxQPlVAl5ggpxakw50jstOLv7e9gflL3DbhQte2Pbv43uVExLjLep0tHJh3qEIXWQYP8toifLiyzxAc+ytcR1ATViSvHnkVMJYoXqUPzAIkc0i/GHNs515NCJBjfy/73efeEZaQzZjebSohJrUK+Idl17Ue573myXajwfzsF2ezur/JdRDUbECEGuqBEJ5372PShFCEQNIVlJ4x13g2hAo8VpE+zBGx3UVTjzO90l4OXE1trj3cD5KRPnCl+bN6CnBkJ8/fSr38mg7yTPKrz6QrgPbxGZ58seSecYKNFK4jgxpt43yf8u2Q9ltmwCFPc7hhpK0Kx+/p1UoxRULNFf1NgoiTkP9mi7RVL2ALpJ4b7qom9dd1SIinBa8PO1rF18Or3r7kGuC2pI90MPPAajz74qDmzg3VT0mwACBa5lPksUp1F1MUrrdQmRW5jM5TymHT2Q9fEEQDQGgyEh4j6WT7W6tPhxwIEqVqiSQosJkgspjhs72mQat53omCfG7JWL/Z399UKQ+zyAGTLxv/ZhH/up5lnPgwDyyecyBC6k4dOW/ZP80IcFjYkhRoyfE8vmhDhFhaeYFuOfLwIadgpIzTSFOOij+Xpfpv1JaLtC8cRBDayHdzFloSv9yPIpBjU3p2BaqR95ZtpMfbq5e80jd8fJy0w35pENDix8O0n7OxkN6myVhq5WkmYpdveBh2kPWOY0FgwK2mSRkZ22JibRYuKALmMQgHsph6fdWD5df7Eh//nxY/oqfqJZAxzc4LzGgBSp+5XtFC54Nm8WDoZWM0XMx8UhOBpK+kTRhxS38zZs6ZYrCyltb/YEhgDt5jcqoQ2JN90V6kErwaBpXXS3zfEMpT+YqRV4qHyw+ceaXxA3CdratucLkpNAQ0BLob/bvqTTE5yZM9O8x9VtYTZsoKRTjkzNO7zc2afS4/3BApqh2g30VKPud4jHeEPYSnl82owBVCXGkSa4UnwVyQTQY/ONz/wl4qhel3ydFN6PHtQbBkEOovZVHPXFC1GrFinUunYLj5oRncJMklnu11v8EZ0Uyl9whcNtXgd1rq/uZFk1PYn0mrcvi1kz9R029rWr2h6C/BEvVBUWEqhnjhSdmPcLSPPwu6exn35v7aLLX6+hmv2ViNZnaBtcVaO+HvIEJVSQ39vn896XmuMEW1JOW/bGaCPRsX7kLDrUY6JcGVpthElfI1uSlwIYiBFtWjwuYiwbo+fNz3F1CYHiIJuh2gx1Me0ln1ME8XHUn5Jr/WdqevYfen10tft7SRcf0U1fhf5/gB5HeFjegNf7xba0PSBtJT64ZacD1FqXULttQpYHLDlM1UGcORofs0rGXbAEJQgEVm/QCL0WC/wMHrRQbOUZGzXUS66vXsyXpBnmhzJFR0X2qmuzee8xo7bP1xa/cIj2WPJIZ+kH143/oWQe0JHOtZ1YU9y4bIFzDwdZmxgfTyDS5P5unJjmDbxoH4A8BJqM5zsJBYVOaL62ADvfrHmW4dT2qfTt1pQGGJ0R8JZf8Tsdz39eLq6rRFS9qgx9IQ+zWkLXlGkOurnZHyW0fmMxT0ddPZnP+bL5E2Ea2N4RZLO0xHI7TjNov1c0T6JhLSHbsI8tmRooPapvTv4afr4vjiIEW9HBKnZhevN7xndZZCYwrggsik7O7wUeojTbFfuJEjCJwiNKpkTlq6ZxnFb7zNQKoL7HCrmRKFYNqXMVkBQCVKYw9NnvngHOnJsjv7vYfwkjP8BxBoEzknbeYHg4DzHn+wJ633alm9ORiEmHqXYhAw8V0oYFZnXcPLATb839tFlr9fQx4QIXfQq8gkNiQdhl9JKoI+HETHjnSdbKtV/AeKxS/9M+1ZmutVa502ORYqCkajullhF1foh5NlPLOQD+HwnC42abqE9NpECNvThH6OlMf4k6alf5HwVH+BjqduTOIUTY7XnCEFzRHN5rINUnaDmtseSh8KYSUQzz0J6QtgKwzkwn+DKRjhnis0fmM7ZzkYEs+4F3hoXR4JLVb8GMDlVFP+gewxGENjlJPU5F1YNxCYfbmlP/rJu4LrizGsO1KwWfCFf2hDWfbQGYJbeidC8P0qN0t8U0td+GRuK3Ko3NruCy/D/wWHcekPKWjTk+dmXkW5Zc5MZnufW+mySdkbM3JQAdG30/EFMrE8egpAQ/MGv2PM4ZEZ6KW6wzbz/ct5vIfAgLg1QV7b8wVd9r4oXrFEfv68eTfxomHptO09mDicTRQ7nwypZzKnIzC8+7Th/acIsMZBlumDc6rWoEb05IOZU9Tz8fZPwjXyxVSGA4QRL3/54VmXXNItMNpklm6kI/Nrkz0iAB4lo2FG2ouYfjTIin4uRc0X9/CjFQPs1SYU00AKmhxEoDeU0FuKajWey2ZXQKITjayAUYXh2ksNN0P+AspI5klvi1MfZVAznANaKA9cuNs/4/sUUXIfcoC+1tPYS9eIwjMJwO+x4MZQ9ZRfoD/p4SZV3f+O8S+WkPXtgF/tAxm37ceIyxf2JqB+GbVI7HkxKEIbDny2WWYL2xLORFrjYJeVk0B+22P3qOc137DNIcjQ2Fqv87S7nSlEenkG4iYPGawGHmAAsuVA3QcRyV1+7zFeHnCvasmSs3q/DVrMQua5d1sQ1ljEQDCeBBHDbZrDV+wmmoajViDqniaZUborAgB+H2CFBLheOb1hniX1xQ7BH2/azkZEVpC8fXG1Qr2V1v+nhJlXd/47xMWV91tgZEcxautode/i7DvrVSexWHdzYmAFrlxtlUu/LZZZgvbEsbbsm9IZSB3pxGxny7LTyINpSjPqUZAMULqgvIhTK2Tg70JxZhjuj2vVx36PPPm6A78l8OTqQllwIF8zqX7BpnaO7Qvgb1tcdMMui947JRw5PCzSbVFgmn3GLig8kTBCSQOqduYmvu6B9I5zn9h1zWNQxdOgF4dQ0qKQbY84Sz6vy9Bi0d3xFrko/fuXXo2iYbYdLqX0Y0BKFWyEn1BhILOkbRvJr+NCPiQQYMpCCDBRri2gwb8rV/bb/ZQEnYvJumt6/1ZxoR5y//gQDPlZ05lCC1PG5gv0gyFGRFKmwEauX+PyApS+7Trx6YGjR8/gQJGn8LUEJC8VXYKObJ3eFsN3IG8VVmgeoK1Fd1XFYIC2qseMAu0qzPNzPp//jcvYIEDmPy9ji+nXoyjjyB7FQB9VjkIU+JXqjYKvFQmVaAeie2yzM6mpXn/tpMLDvr3HyOSG4hVKHpOu9tEjjI2mMYEVaVqZjJYh7W4QBx2CyFVATWq5VMp9lCAF3wfGY7iNDeeG5BCiPYhQ086EqaK6t2znN+UmkrUrAHOHfiPcOTD0ZfumYDebmUGYtXc1bRwDo/5B92F51fuPkmUUCTxjZmTMRsFhvR6Lp3pXvpIhYJD3EDnM0jiW+jHNaG0TXhUg6Rg/59Yls9aaVS7zWNRkY8cPsyXV8yVl2b4Z/OnQnxUEyhMBKuXIoe5gL5VOmdx01wIukS/+48KQ1K4BAIbCCoJ0ZFI03M1dLcyQ/W1u4l5eGd2u2hn9Hw1xEtFk4nApwFw2Udhn2bNDAjpo/kefRUinG8Iwcz7zMSdFW9ruL+hl/uJ1YLRcxgKOdvePu8APH0IBl++RvjzFPzM0s4x//cxtGwFDcbDEAyCXzWCx/E2iMpg2sHtTtNVCIoVmfKeX3LiOnQGGOE0k5ChNfxfhVn8A8fz5YvH+fY3BFvVWKtX/GPjF8AHv0YIPZKK3toKFECUKADTT3JXpdced/I82CJMFPCSMlPbc3a1xcNyRuNQ+FmfShQ9rsz+pAi8prSNeivnQaDr/yb0v941sirRb0/U+czsJ2aZCEJZb1tTffO5YzXo5Xr5dc88iSm4xhO1N04D8Q/L1WWm/xmos9jAThz7x0bIPqLWZHyewlpsCPoDrsRMzjzFUSmcecAyEtNE5otOk33F6YWTnBcTySFH39dWkT1nccP+kPIQOzswcYbYLc1zZuw5voOMxqENm91P9wmxa4qaS4VevHYWmodj8XrII4o/qm8tjU8+EmVd3/jvEvm/IcVmlbXjhVSwOv0c07/F+3z/AHmCKJ/b/VssswXtiWWUljfHToNIQNHdcFrscD/M2fD/qH/9j+s/aU6V0KL+hpJ5d6qQACdj85XbLA0pRlKSoT4iw9H0ae1DsEfb9rORbnqg/w+4GqUBCdy3ANPJK3CwW9692a3oCf7idWC6Jup6GsYAR7IDs5ugYzChDn79FsvDpQFEamrOKBvp6CVHoNjyunTgJirB27MKIKVx3M6xSOO8SOtJxfL60h0kI/LaT8YYgFAixVwPYdTkav6Z2SgTZgKGdPPe3DqAO7biyYzyfD2c4XRcVaQPzL43jz34ExqQiC1kd5auhYN9+yaGzn1Wh2n3v8F2roXGMylaf/HJIcSdgqd8Ua2qZmWv+05pBTFAq1Xhywv2zOJZYfK05tJul/qkYnrpIvqIeD+akufkkViTju0Urb6X745LR7V4MfS+LpnFsw4EetKlX/63G0CUXHNUJsoHBtcOgrX8t+UwMLRb2IuTFrWiQMcghbFeuQXSCn52vFuoAIymAxLSSKXEdqzz583/QZbxQQL/iSbY+bS0f5fjCyZh2gIIv7tXM/Px3jeIRqHAMpkHoaK+U6hqpsgYfoDPI0xMOUSFmPCN9IanpnUaExax8bFuS9ToPZh9neS2nGqFmbaAVfdPrs1OnOYAqkPLj0O4Rny3F7cPdoZ4cQk+dkQ/jt/tVUIyzVnnvZTIyxSFsJhVqL4t+uSFiSek6KrrbCYn17vuyLkPASkB2ZCJOh7w8U18WvzB8P9hNakVWk8lH1tZRt2q0eC4WDdJoezv/z0DIESx6mu3NPDHUFiehkw7nTm7t8X/xOB5G0BPmHWpobHEGr6xqo8I5JNguakFsjeVwZK0ykEnmz1gJ2kXUzQdVhOPtXMV7D04Hh0MsK9YuQWfUCsL1yOOX69cJdK/dIC41rTRwwTjaZ1IhFaaCIJV73QgxLvX/eF7S7HAaaiJGgDu6ghGyN2K5nF1MIXcOdVl/4ykJDbWk9V9LuaBGjBj/dDnR4oapie9r7AhvDrkZgYh1KmeQQOxZOHUSYYP8mwq2aJl05/5YzPhQ6v6x3+DkcfQgMSI8F6fqs9t3/fUaI5VilVxfROZdL81L1XIFLqhehGyf7F3CDELXFdInuny2RGP9d3ltPdZEKYEuyi/8KDgt5FYitZuKn+p4sa8eaIm1NwYlJlgPNFzt1RJpXhcgRvfVR2MNg9Vrfpnw2fCJHFWdEENS+Fcl6aiMixc0JYdicYO+Z7ebysOCZQk1EJm4+Q3oKGkiw4tvTFoTztukkYP1Vvr8XFwAOc7bpIPSxuEJtJeT/Iv2c9herZ3Nzk9nwWoBsarhJiuBH0woR2udxjQtEPmpKdvhEj+qmQHNP0ei1L+TH2xPwnpW95DzW21ZCJhzCYNtQgsPDN55B7btjC4Yval1eS8bjPvNnSmzFM5JdXkvG58MZbz60F77nJ7Pgum3jHcNUEb5VGyia4/UeUCv8IcyVE50RwhmvFfR2MNg9VrfpCru+i8y6X7a1KVxN9ep/ZrzmnCaGZzfmBxhrhmx9Kd1U4z2+vxclvciehQolvznvdjh3827VFgD06Mxlb+jEsXMIiZ0Gceuk+hNCp3sS+dlTh1m4amkOz2hgmJgmduoJXd/exvJMvYsqGX28GBw1z0cXcDp3QZt2GX6YCqmNC/a0Y0vtbYjUN9+nL4pCbYZ9Cewk6vJPdZEKYEuOAV1zxnFxYDigTxSk7WO9bJGhwFVno+Yz4WT21eBq9z5Efv+32hPWfoHrS7F/iQICdbbF/BNpjbX56Myiuf+KL7f+7XL42tlB4mfu/oszPUgehvB/TD2EI/DJy5N1SV+SEiGAMe82IO7kSkGJ8T5+2yYdUMV+Slm7R1G0sSvhAEVqxr/RjI7LkPwQrKQtT2QGGX4fqR8WPi9PeGfczJ1xCyvnCjyGiscX+vE9Qas1XW0mgmCtqgMme0vTxJ3pEBzD3NVDRU2996IfObq1gblMnFodU5YcD76e6IZ1RF1w4yOssyYZIJlocsoxxqEmdBnHrpPoTJN0hC2UIhRxmYRBEUuzkeTmiS0oWuuYN5B9llERVv8Yoe6YHf8dzFk9D076Zq7X+oDuk5jAyuiu/Ir2D/XkklFarBUTJCU4AjSyWdMcmhCPYUKdxRZ1tfvB6LL6J9J7bIOQBqfrvgQFeGFCccc8LjsFPeCNk3EgGd4GSJc3Ci7oREF9p2/8h+5fjKAjiSaRW2TvxVxaA9eV902h4wh6OrAOKBPFKTnOUVwX/R89LuxiOdRqC1Hex0dK2L3i4Xus/QPWl2NJUO5Rwl1n9D9ctfRYtK8DdR7ip4lpLMs1wVr8dUeX6Q4wwBdt/7tcvja2Ypu55F52ueUt78jGPNh2GI3BRfteWUitLv/7Irp9zupRQMPWVqN5agS61aRi/KpwvP5MbMR11vhungQ6dd1ezcShC0NuE4b2CrJF2HB5obSE69y7KJj2wXsk/aHE328ht0Y4uVhOXgFISmmlxjjFTZGhw0PAasZKC0Mh1aMzUIXVouH/nWcdhp9/nAwMVUT6tFsX3cya53UrMdb+hYxqvJ8L+gApMi2dgwQUDnYkOda1dVsRZsXKXLX8iMCUspNwTFbh2JkwawlAG1w6Ctfy5bLawOXL8v2o7EQs3J/H2Yapjvmo+VAeiTqhR3g6TIWCVQ55H+dmCWO7x8ifAvm/QTJlo/MYoUXqmFJ4kll3YrRX/4PnY7E57LCzEg4+1fTsXZGJVJmfcPSoQpyS6vJeLyjFwPvCQ2e3xRfbqZ6IbjrapA8THqNR2M7yctMEyO/76jRyCBAketbPOXIs9p3d5IrBTRv4PZ71kTRAZCQ0tf5CsN1DgAwuCvg7DoAAahnJLg1gtDmJ781ePZKzM1R1Gyg2nrSD2cVeOqOyW0MbpsTvoSSrIaRoVN2VP+suItBd+9qFlWIHS0NCYN8AFUPriIFeEZudVLrJZImdQ5z9sQu05EOrGZDnhigp/fq+Le1neCvZjjYYTcqc2AkW+na7NvQ/Z/Kaw9v7qRU0Z+A8nAWos2Dr1bDFDyF8mW9UMYOS7X0kYaKUIdp7ZSQ5AO4Eh1hzmgCtIw5mzLVScj0+32QSgTb5p0HGCK1eGqIFkvV7iqcKjdNW3uw9e7pMLGki8YZZHosCXKy2mLSOx0MOfZ73w7BA9hzL9THkR59mPiFu6hPaf8nbSJmSGqxBVEP3paFGWapQq0XknT6jKn4QD9xx2KsUzhI1VmKRo2twU3JOD54ihIGqgiJn8Xmm3/Ic9jSErbYjp5//nuQmpwqNMo6GbhH4vv3Bq8xMqCRimogWTx2WTkNsMjNCGUvDH/TttuY5Dxha1I3L8AZHXZivB0/mjbytSIq0knh6lrTjmwTUnrfsLQe0XZOBPZtR2fnaWn8c43fDHYukSXDVEySPkeBYhvUg9FTX+fiAIF1YfJf0apLsO4ryehhjdg9kzXB7YYV5iXyWT1dDF7Zd41ZMmBUR8O90z/F/nRpzNa/N9Y9o8V8ucUqXJkRALZKn3AFDtZQfiscUeuwLYAnHNNXUvOp6TImE8v4za5XEZozuiVrCja8P1OnLmNzWH50tnfwzvr2lw6EXsCnqLEQQsg0lTeT611+BvAzcdyPpTXLZkYJgXeYZEY+9pCa1fXFB6gWdbNwo9Fr9cljDMwjxMUdTB1P9C77gFswUe5QxpXSgm2FP1ov6S+fzA/zM5JbNwUU7e9Wut0/95IHzhW3u3dT5Tjp3EFfZTyHYbxElFfB4zbGWcXDHabSPBvYX4TFlerGBYLPCdVg7P14SmW+BXULcReiyvBQDZyWlWc10jWgwwriDYNv1+x3cd0cYJqbKriT8Uyj0WSolK5VduzZL7ytzSaA0SFKmOGTuJjW1aTi+u3FnvQ8LPChppiN/8wkKcXAjLG/JyLvTbLwAPlbqWnhtxst84VP3Ac/qS8HUT6nhXTNwjsD6vfPRm0dBsAdEhppYwi94fbtb+S/EZYajB63NM7uLm32l5c3hqXl8czTHw3av8Vz/rwAudqQakqWaPirN551yiSHgl5nvOK9pASO/LzeXveB5Qaf55AthxR5Yx4k7UUbStHK29mEevXLJuZ25vebFHJBNDwZgSiLUlD38S6IXF5w9cnV+5TYEAF3CuYIqXYS+5ZnCvSLag4w0DpL4xIQx/x7f3vOX+9Io1jb3NO8U3/5u67jgjqBx9F9bv9HcvHElqguyOyDAcrcTsmTMIY86qe4TR20YrmuI7M0PgvP4SH24GVeMdwDGvW4KEewWLmA6ibS4bcn0M3atfwWkq/n24Fn3l+wqeYoP4Et6r1N2i6X7Cp5ig/gub+IfgVp7z4QYTxCpIZNbNUpWQsFmnWZWHIudbM6PEOEKBTdxquLcYBV16RguEe6BNzgEgJI2f0BGVMkh1iaGx1+zezumA69r6gdYhO7KlBlYA7UyUbmh7qdHlQ8Wt7vHJMZXNjA7FqSLyq2TR5UBVrf/q0ldWTdFPZv47r8n4i8NoxG1Jb6QYpySSmGX3Dx9kUE4pJn4wVz5luYlHEPgUgYqgUr2THUb8Ew4s3mOirU46RW9ogsHlprwQEfER2xzZSHR3Pr4Cei1dVaa/h+Hsp9jYPAHM95JaRs/4zJ6I09P7YX8Fw3CLdTLUb3hECQrQbvqzA77sNSFe8i+eYs9th9Zj71iAENFAdAFayvt+TiSaeIrAviDGHrAAAAAAAAAAAAAgD+n/KZafoMCAaEZjINCAmTYAAAAAl8WN6DuXzuuU7NcVgBd5QQ2I+TAAADlJ2gWQ3INlB8jp4AragACEPsrw3hlPfS0EhgDzrnZeoHwB7rAlyITlRS/d6xoACJcVrw5Q8OeQnOudmBqxFAgBa+AaF6EZvHKFYiAAAAAAA=)

1.  **Update Firmware**

```
cd ~/klipper/ && make flash FLASH_DEVICE=<Your Board ID>
```

Note

*   Replace `<Your Board ID>` with the actual queried ID.

![](https://docs.3dmellow.com/en/assets/images/katapult1-fae974447ad1f68b97db907a1e3f32ba.webp)

---

## Solution for Flashing Wrong Firmware

### Method 1: Quickly Enter Flashing Mode

1.  Power off the board.
2.  After powering on again, **quickly double-click the RESET button**.
3.  Re-enter flashing mode.

### Method 2: Re-flash Katapult Firmware