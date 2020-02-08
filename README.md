Xavier 安裝懶人包
==================

此安裝內容如下
----
1. Xavier刷機教學(內建Cuda10,OpenCV3.3等)
2. Ros melodic
3. Tensorflow-GPU
4. Realsense(含ros-realsense)
5. 預計更新 OpenCV3.4 UP
Nvidia-Jetson-AGX-Xavier-TX2-JetPack-4.2配置流程與基礎設定(刷機流程)

>>2020/02/09 更新
1. OpenCV3.4.3安裝
2. 如何掛載SSD在Xavier上


----
# 設備需求
1. Jetson AGX Xavier
2. 螢幕、網路線、鍵盤組
3. 一台Ubuntu電腦
>備註:Ubuntu中開此github上我們有遇到直接複製指令下來，貼上terminal時，指令會失敗，所以後面如果有遇到要打指令的地方建議手打。

# 環境設定
		
JetPack 是用來快速初始化Xavier、TX2的軟體，由Nivdia官方發行。
在Ubuntu的電腦上，下載 Nvidia SDK Manager。

<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a1.png" width="700" height="500">
		
下載下來的檔案，是一個.deb檔，如圖所示，接著在terminal打以下的指令進行安裝。

`sudo apt install ./檔案名稱`

>備註:terminal的路徑要正確；檔案名稱為你那個檔案的名字
>>圖片為示範
<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a2.png" width="700">

安裝後就會有SDKManager這個軟體，點開後如上圖。
點開後的登入介面入下圖，這邊的登入帳號是Nvidia的帳號，要先去官網註冊。

<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a3.png" width="700">
<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a4.png" width="700">

登入後的介面如上圖，Host Machine請把它點掉，Target Hardware就選擇你要刷機的平台。
下圖為把紅框那邊溝起來就可以繼續下一步了，它會開始下載檔案。
>備註: Host Machine如果勾起來，它會在你的電腦上幫你安裝其他檔案，強烈建議不要。

<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a5.png" width="700">
<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a6.png" width="700">
下載完，會跳出這個畫面，如果你是第一次刷機，請將Automatic Setup改成Manual Setup。
改完後，先放著，開始把Xavier拿出跟電腦連接。

<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a7.png" width="700">
<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a8.png" width="700">

Xavier接上電源後，拿附贈的傳輸線，USB頭接在你的電腦上，TypeC接在Xavier上面。
Xavier側邊有三棵按鈕，先壓住中間的按鈕，在壓住左邊的，等待兩秒後，兩顆一起放開。
這時候就會看到Xavier跟電腦連接上，如下圖。(出現Nvidia Corp就表示連接成功)

<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a9.jpg" width="700">
<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a10.png" width="700">

請確認你兩個平台是用同一個網域接出來的網路，我兩台都是從同一個數據機接浮動IP出來。

<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a11.jpg" width="700">

接著電腦這邊按下Flash就能會在開始把OS裝到Xavier上了，如上圖。
裝完後會出現，下圖這個畫面。接下來要先設定Xavier，按照Xavier顯示的畫面開始設定它。

<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a12.png" width="700">
<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a13.png" width="700">

Xavier一開始的畫面如上圖，接著就開始設定，設定好後，它會開始安裝，安裝完後會自動重新開機。開完的畫面如下。

<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a14.jpg" width="700">

登入進去後，先輸入sudo jetson_clocks，這個指令是用來開啟風扇的，每一次開機都要打一次。
電腦端這邊輸入你剛剛在Xavier上面設定的帳號密碼，按下install。

<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a15.jpg" width="700">
<img src="https://github.com/SamKaiYang/Xavier_install/blob/master/image/a16.png" width="700">

# 刷機常遇到的問題
如若在上一步失敗，那最大的問題大概就是出在你的網路問題，可以在terminal 打ifconfig 看一下你兩個平台接的網路，因為它在灌OS的時候不會有問題，通常都是下一步安裝CUDA、Opencv這邊會出問題。
如果真的失敗可能就要重頭再來一次，然後確認一下你的網路是否有連接正確，通常這一步都要多嘗試幾次才會過。
# Xavier基礎設定
每一次開機都需要輸入下面這行開啟風扇

`sudo jetson_clocks`

接著是調整功率模式，下面第一行是查詢目前的模式，第二行是切換到效能全開的模式，這個只要設定一次就可以了，重開步會跑掉。

`sudo nvpmodel –q`

`sudo nvpmodel -m 0`


# 安裝相關套件
建議安裝順序
1. 安裝ROS
		請詳閱[install_ros_xavier.md](https://github.com/SamKaiYang/Xavier_install/blob/master/install_ros_xavier.md)
2. 安裝Realsense
		請詳閱[install_realsense_xavier.md](https://github.com/SamKaiYang/Xavier_install/blob/master/install_realsense_xavier.md)
3. 改USB頻寬
		請詳閱[usb_xavier.md](https://github.com/SamKaiYang/Xavier_install/blob/master/usb_xavier.md)
3. 安裝Tensorflow
		請詳閱[install_tesnsorflow_xavier.md](https://github.com/SamKaiYang/Xavier_install/blob/master/install_tesnsorflow_xavier.md)
4. 安裝OpenCV3.4.3
		請詳閱[install_opencv3.4.3_xavier.md](https://github.com/SamKaiYang/Xavier_install/blob/master/install_opencv3.4.3_xavier.md)
5. 安裝SSD在Xavier上
		請詳閱[ssd_install_xavier.md](https://github.com/SamKaiYang/Xavier_install/blob/master/ssd_install_xavier.md)

