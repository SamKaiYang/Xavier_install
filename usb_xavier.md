Replace xusb_sil_rel_fw_Xavier
==================

步驟1:
---

**複製[xusb_sil_rel_fw_Xavier](https://github.com/SamKaiYang/Xavier_install/blob/master/xusb_sil_rel_fw_Xavier)此檔案到隨身碟內(自備一個隨身碟並連接在Xavier上)**

步驟2:
---

**備份原始文件**
```
sudo su
mv /lib/firmware/tegra19x_xusb_firmware /lib/firmware/tegra19x_xusb_firmware_ori
```
步驟3:
---

**複製xusb_sil_rel_fw_Xavier到路徑/lib/firmware內**
```
cp <請輸入你的隨身碟路徑>/xusb_sil_rel_fw_Xavier /lib/firmware/tegra19x_xusb_firmware
```
步驟4:
---

**重新開機**
```
sudo reboot
```
步驟5:
---

**此為確認步驟 可省略**

The fw timestamp should be:
        
		root@tegra-ubuntu:/sys/class/tegra-firmware/3610000.xhci# cat version 
        
		3610000.xhci: Firmware timestamp: 2019-07-16 08:23:26 UTC, Version: 60.05 release

Remove all the USB device and confirm that xhci enters ELPG
	
		Check "tegra-xusb 3610000.xhci: entering ELPG done" in kernel log

步驟6:
---

**增加falcon clock頻率**
```
sudo su
cd /sys/kernel/debug/bpmp/debug/clk/xusb_falcon
echo 1 > state
echo 408000000 > rate
cat rate
```
		確認顯示是否為408000000

**Fininsh!!  即可連接Realsense看是否還有溢出問題**
