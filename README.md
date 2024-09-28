# Speedtest-Server-Ookla
Speedtest Server Ookla

1. Spesifikasi server sebelum melakukan installasi harus di sesuaikan dengan acuan pada halaman : https://support.ookla.com/hc/en-us/articles/234578628-Speedtest-Server-Requirements
2. Install Ubuntu Server, untuk langkah2 ini di jalankan pada ubuntu 22.04.5 https://releases.ubuntu.com/jammy/ubuntu-22.04.5-live-server-amd64.iso
3. Siapkan IP Public dan subdomain untuk speedtest server
4. Akses Server (Via SSH atau yang lainnya)
5. Download file Installasi ( bisa langsung atau di arahkan ke directory tertentu)
##
    wget https://install.speedtest.net/ooklaserver/ooklaserver.sh
pada perintah ini file yang di download berada pada directory /home/

6. Update Hak akses Script Installasi
##
    chmod a+x ooklaserver.sh
    
7. Install server daemon
##
    ./ooklaserver.sh install
    
8. Jika sudah selesai silahkan cek pada browser
##
    http://Ookla-Speedtest-Server-IP:8080
![image](https://github.com/user-attachments/assets/aa5d51c7-c68e-4b54-ae9c-f7c4224c217f)
Jika hasilnya sama serperti pada gambar di atas, maka proses installasi sudah sesuai.

9. Edit file konfigurasi Speedtest Server Ookla
##
    sudo nano OoklaServer.properties
    
10. Edit perintah2 seperti di bawah :
##
    OoklaServer.useIPv6 = true
    OoklaServer.allowedDomains = *.ookla.com, *.speedtest.net
    OoklaServer.enableAutoUpdate = true
    OoklaServer.ssl.useLetsEncrypt = true

11. Restart Ookla server
##
    sudo ./ooklaserver.sh restart

12. Selanjutnya test server yang sudah kita install pada website : https://www.speedtest.net/host-tester
13. Masukan domain yang sudah di pointing ke IP Speedtest Server Ookla seperti pada gambar di bawah
    ![image](https://github.com/user-attachments/assets/189e0614-a6de-4f11-bac0-a5ab3c095ce1)

14. aktifkan auto-start untuk Speedtest Server Ookla 
##
    sudo nano /etc/systemd/system/rc-local.service
##
    [Unit]
     Description=/etc/rc.local Compatibility
     ConditionPathExists=/etc/rc.local

    [Service]
     Type=forking
     ExecStart=/etc/rc.local start
     TimeoutSec=0
     StandardOutput=tty
     RemainAfterExit=yes
     SysVStartPriority=99

    [Install]
     WantedBy=multi-user.target 
##
    printf '%s\n' '#!/bin/bash' 'exit 0' | sudo tee -a /etc/rc.local
##
    sudo chmod +x /etc/rc.local  
##
    sudo systemctl enable rc-local
##
    sudo systemctl start rc-local.service
##
    sudo systemctl status rc-local.service  
##
    sudo nano /etc/rc.local
##
    su ookla01 -c '/home/ookla01/OoklaServer --daemon'
Sesuaikan username yang di gunakan, pada contoh ini menggunakan nama ookla01
