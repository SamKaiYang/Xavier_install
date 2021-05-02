VNC遠程控制
1.安裝vino

```bash
sudo apt update
sudo apt install vino
```

2.設置Enable VNC
```bash
sudo ln -s …/vino-server.service /usr/lib/systemd/user/graphical-session.target.wants
```

配置VNC server:

```bash
gsettings set org.gnome.Vino prompt-enabled false
gsettings set org.gnome.Vino require-encryption false
```

編輯org.gnome,恢復丟失的“enabled”參數,輸入一下命令進入文件，將下方key內容添加到文件的最後面。保存並退出。

```bash
sudo gedit /usr/share/glib-2.0/schemas/org.gnome.Vino.gschema.xm
```

在schemalist最後一個key後加入下面代碼
在這裡插入圖片描述

```jsx
    <key name='enabled' type='b'>
      <summary>Enable remote access to the desktop</summary>
      <description>
        If true, allows remote access to the desktop via the RFB
        protocol. Users on remote machines may then connect to the
        desktop using a VNC viewer.
      </description>
      <default>false</default>
    </key>
```

設置為Gnome編譯模式

```bash
sudo glib-compile-schemas /usr/share/glib-2.0/schemas
```

現在屏幕共享面板在單位控制中心工作…但這並不足以讓vino運行!所以您需要在會話啟動時添加程序:Vino-server，使用以下命令行:

```bash
/usr/lib/vino/vino-server
```

/usr/lib/vino/vino-server
3.設置密碼
(‘thepassword’ 修改為自己的密碼)

```bash
gsettings set org.gnome.Vino authentication-methods "['vnc']"
gsettings set org.gnome.Vino vnc-password $(echo -n 'thepassword'|base64)
```

4.重啟機器

```bash
sudo reboot
```

5、設置開機自啟動VNC Server

```bash
gsettings set org.gnome.Vino enabled true
mkdir -p ~/.config/autostart
vi ~/.config/autostart/vino-server.desktop
##將下面的內容添加到該文件中，保存並退出。
[Desktop Entry]
Type=Application
Name=Vino VNC server
Exec=/usr/lib/vino/vino-server
NoDisplay=true
```

6.連接VNC
PC端打開vnc viewer 輸入IP地址遠程控制。
