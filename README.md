# Automated Mushroom Cultivation Environment with Raspberry Pi Model - 4 B

## Problem domain & Solution 樹莓派專題-製作自動化香菇栽培環境解決方案模型
菇類產業發展至今已有百年歷史，是為餐桌上常見的珍饈，更是各國料理不可或缺的食材，其富含豐富的蛋白質、維生素、礦物質與膳食纖維等營養素，目前我國各式菇類產業整體產值已逾100億新台幣，為我國農業發展之要角 。近年來勞動人口減少及人力成本提高菇類產業面臨的問題，其中太空包製造需要大量人力，行政院農業委員會自106年起推動智慧農業計畫，藉由智慧農業相關研究，將自動化、資通訊、物聯網與產線設計及機械製造等技術導入菇類生產流程當中，幫助產業解決問題並增加產量及工作效率，以提升我國菇類產業之整體產業環境。據報導，經台中市新社區農會統計，台中市新社區香菇栽培面積達200公頃以上、供應全台香菇65％以上，每年為新社地區帶來近25億元以上的產值，剛好我任職於這個地區也對這產業有些許認識，也順道帶大家介紹一下這個產業生態。
本專題是基於樹莓派的香菇栽培自動化系統。對於香菇栽培農場，溫度和濕度是需要控制的主要敏感因素，因此需要經常持續性監測這些因素數值，以保持合適的濕度和溫度平衡。
本次是以一個普通香菇農場的規模來試驗，占地約為1,500-2,000平方公尺，概估約有30萬個太空包，如此寬廣的空間要控制溫、濕度是相當困難的，換句話來說是要耗費相當高的設備資本才能辦得到，這對一般種植香菇的菇農來說實在非常困難，所以本次專題只是藉由嘗試透過樹莓派(4B)安裝溫溼度感測器(Sensor)在香菇農場內，藉由收集相關數據，未來可能採取連接外部資訊(如氣象及降雨預報等)些微調整內部、外在環境因素，長時間觀察分析數據改變情形，進而找到可控因素以提升整體產量。

## To complete this project we have used the following equipment(本專案使用設備如下):
* Raspberry pi model 4 B
* SD memory card
* Ethernet Cable
* USB Cable to power Pi
* DHT22 sensor 
* 4.7 K Ohm resistor
* 4 - switch Relay module
* Some male-male, male-female and female-female Wire
* PC/Laptop to view and control Raspberry Pi 

## Step 1: OS for the Raspberry Pi(作業系統安裝)
We have installed Raspbian OS to run on Raspberry Pi . First extract the OS files into pendrive by [win32diskimager](https://drive.google.com/open?id=0B496SaFqKMZCcVVJZ2NxcUp6Ujg) and Set SD to Raspberry Pi and powered it up. Raspberry Pi started to boot up. We created a file named “ssh” without any extension into bootable memory card to enable ssh into Raspberry pi. 

## Step 2: Connect Raspbery Pi to PC(將樹莓派連接到 PC/NB)
先行外接鍵盤及滑鼠，以便於操控。接續介紹VNC軟體
VNC是一個遠端桌面的軟體，分成我們的VNC Server(Raspberry pi)，與VNC Viewer（筆電端）。
Raspberry pi：這邊很簡單，跟上面開啟ssh權限的路一樣，只要進入偏好設定->Raspberry pi設定->介面tag，把VNC啟用即可，這時候在桌面的右上角就會出現一個VNC圖示，我們打開它就可以看到VNC CONNECT by RealVNC，只要在VNC Viewer輸入上面出現的ip address（這邊是192.168.10.7），就可以直接遠端控制Raspberry pi了。(可參考https://ithelp.ithome.com.tw/articles/10235452)
在上面輸入ip address進行連線，這時候會需要輸入Raspberry pi 的登入帳密，請輸入昨天在設定時的密碼，如果帳號為預設值的話打pi即可。
這樣就可以連線成功，可以遠端控制你的Raspberry pi桌面了。

## Step 3: Connecting DHT-22
* Make sure Raspberry Pi power is switched off.
* Breadboard is used for the following connection.
* “+” (VCC) pin to Raspberry Pi GPIO +3.3 V pin.
* “middle” DATA pin to the GPIO pin.
* 4.7 K-Ohm resistor is set up to “+” power pin and DATA pin
* “-” (GND) pin to GPIO Ground pin.

## Step 4: Connecting Relay Module
* Make sure Raspberry Pi power is switched off.
* Breadboard is used for the following connection.
* VCC pin to Raspberry Pi GPIO 5V pin.
* Rest of the four pins to GPIO pin.
* GND pin to GPIO GND pin
* Power on the Raspberry Pi

## Step 4: Install Libraries and Packages
#### PIGPIO library install
```
cd ~
sudo apt-get update
sudo apt-get upgrade
rm pigpio.zip
sudo rm -rf PIGPIO
wget abyz.co.uk/rpi/pigpio/pigpio.zip
unzip pigpio.zip
cd PIGPIO
make
sudo make install
sudo pigpiod
```
Downloaded “DHT22 AM2302 Sensor” from
http://abyz.co.uk/rpi/pigpio/examples.html#Python%20code
Unzip and pasted into the folder where main script will be written.

#### PHP5 install
```
cd ~
sudo apt-get insall php5
```

#### MySQL install
```
sudo apt-get install mysql-server
```
Note: set password: root 

#### PyMySQL install
```
sudo apt-get install pymysql
```

#### Phpmyadmin install
```
sudo apt-get install phpmyadmin
```
Note: set password: root and choose webserver “apache”

#### Apache2 server install
```
sudo apt-get install apache2
sudo nano /etc/apache2/apache2.conf
```
At the bottom write “Include /etc/phpmyadmin/apache.conf”. Save and Exit by CTRL+X
```
sudo /etc/init.d/apache2 restart
hostname -I (to get apache server IP)
```

#### Execute Script
Copy and paste "Project" folder to 
```
Directory: /var/www/html
```
* Open browser and go to "apache2 server IP/phpmyadmin" or "localhost/phpmyadmin"
* Login to your phpmyadmin as user: root and pass: root
* create a new database called "myDB"
* create two new table called as "myTable" and "currentTable"
* "myTable" contains 5 rows serially such as "date", "time", "humi", "temp", "status"
* "currentTable" contains 2 rows serially such as "cdate", "ctime"

Now open terminal and write
```
cd ~
cd PIGPIO
make install
sudo pigpiod
cd /var/www/html/project
sudo python myscript.py
```
Humidity and temperature will start appearing on your terminal. Now open your browser and type your "apache2 server IP/project" on URL and hit enter. Data will start showing on webpage. 
#### For more details with images, checkout the PDF file given above. 
#### Or watch the video https://youtu.be/R31BI1YEOBc 
#### Enjoy!
