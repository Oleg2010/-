1. Заходим на сервер по ssh и открываем файл на редактирования

   ```bash
   cd 3proxy/
   nano 3proxy.sh
   ```

   Откроется файл примерно такого содержимого:

   ```bash
   #!/bin/bash
   
   echo daemon
   echo maxconn 100
   echo nscache 65536
   echo timeouts 1 5 30 60 180 1800 15 60
   echo setgid 65535
   echo setuid 65535
   echo flush
   echo auth strong
   echo users admin:CL:pass
   echo allow admin
   
   port=30000
   count=1
   for i in `cat ip.list`; do
       echo "proxy -6 -n -a -p$port -i81.177.3.96 -e$i"
       ((port+=1))
       ((count+=1))
       if [ $count -eq 10001 ]; then
           exit
       fi    
   done
   ```

   Меняем пароль на строке 11 после `CL:`

   

   Если нужна привязка по ip, то строки

   ```bash
   echo auth strong
   echo users admin:CL:pass
   echo allow admin
   ```

   меняем на

   ```bash
   echo auth iponly
   echo "allow * 100.100.100.00"
   ```

   

   Можно привязать неограниченное кол-во ip (в переделах разумного)

   ```bash
   echo auth iponly
   echo "allow * 100.100.100.00"
   echo "allow * 101.101.101.01"
   ```

   

   2. Сохраняем файл 

      `Ctrl+X`

      `Shift+Y`

      `Enter`

   3. Перезапускаем 3proxy и ребутим сервер

      ```bash
      chmod +x 3proxy.sh
      ./3proxy.sh > 3proxy.cfg
      /root/3proxy/bin/3proxy /root/3proxy/3proxy.cfg
      reboot
      ```

      