如何安裝SSD硬碟至Xavier上
==================

Xavier共有16G運行內存和32G eMMc flash 。

看起來倒是挺多的，但是裝上系統，ROS，CUDA、Protobuf、bazel，Qt後基本所剩餘無幾了。
現在我就教大家如何將硬盤分區並掛載到/home目錄下。

我用的是威剛 M.2 nvme 256G固態，接到板子上的M.2 nvme 接口上。

如何將SSD裝在Xavier上請看此影片
https://www.youtube.com/watch?v=x0TBTYw7HKs

>>以下步驟適用於若你使用後，發現又要重灌了，以下步驟可以再將SSD洗掉並建置在Xavier的/home目錄下


步驟1:查看硬碟所有分區
---

**查看硬碟所有分區。 會有一個/dev/nvme0n1就是你所接入的硬碟**
```
sudo fdisk -lu
```
步驟2:對硬碟進行分區
---
```
sudo fdisk /dev/nvme0n1
```
**在Command (m for help)提示符後面輸入m，可以查看支持的命令。**

**在Command (m for help)提示符後面輸入n，執行add a new partition指令給硬盤增加新分區。**

**Partition type: Select根據自己的情況我選擇了primary主分區。**

**出現Partition number(1-4)時，輸入1表示只分一個區。**

**後續指定First sector,默認起始地址為2048，結束地址為：****，不輸入數字按ENTER，將填入默認值,我個人直接Enter。**

**在Command (m for help)提示符後面輸入p，打印分區情況，可以看到已正確完成分區。**

**在Command (m for help)提示符後面輸入w，保存分區表。退出。再次輸入下面的指令**
```
sudo fdisk -lu
```
**顯示/dev/nvme0n1p1則表示分區完成**

步驟3:格式化分區為ext4
---
```
sudo mkfs -t ext4 /dev/nvme0n1p1 
```
步驟4:掛載硬碟分區
---

**先把新硬盤掛在一個臨時目錄下**
```
cd /mnt/
sudo mkdir home
```
**掛載到/mnt/home**
```
sudo mount /dev/nvme0n1p1 /mnt/home
```
**查看,此時/dev/nvme0n1p1 的路徑為在/mnt/home下**
```
df -h
```
步驟5:替換原home目錄
---

**把home下的東西拷到掛載的目錄下，備份**
```
sudo cp -a /home/* /mnt/home/
```
**把home下的東西刪乾淨刪除後整個桌面的menu、任務欄等都沒有了只剩下終端窗口,所以不要害怕怎麼顯示變奇怪了。**
```
sudo rm -rf /home/*
```
**卸載硬碟**
```
sudo umount /dev/nvme0n1p1
```
**查看卸載成功**
```
df -h
```
步驟6:設置開機掛載
---
```
sudo gedit /etc/fstab
```
**打開文件後在最末尾增加一行,如下**
```
/dev/nvme0n1p1 /home ext4 defaults 0 1
```
**然後保存"Ctrl+S",退出**

**查看/home是否被掛載,此時並未被掛載/dev/nvme0n1p1 的路徑為在/media下**
```
df -h
```
**掛載/etc/fstab中未掛載的分區**
```
sudo mount -a
```
**查看,此時/dev/nvme0n1p1 的路徑為在/home下,即成功**
```
df -h
```
**最後重新開機**
```
sudo reboot
```
