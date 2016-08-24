
iconv -f gbk -t  utf8 /mnt/ubuntuwifi.txt > /mnt/ubuntu1.txt


2. wifi

iwlist wlan0 scan|grep "ESSID" |less 

Input,to enter the interactive mode
```
    wpa_cli 
```


Input,

```
add_network
```

you will get a number 0 ,or you can call it NID


Input,

```
add_network
set_network NID ssid "dns"
set_network NID psk "f708f708"
bssid NID "BSSID"
set_network NID
enable_network NID

```


Input, to enter dynamic IP allocation
```
dhclient wlan0
```

hello
























