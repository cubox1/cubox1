-

Так как GitHub прислал письмо оплатить размещение сверх 1Гб, то прежнюю репку пришлось удалить.  
Папка boot: https://www.dropbox.com/s/0v7ic4j6atnlqo4/cubox-installer-snapshot29012012.zip?dl=0  
u-boot-\*.bin: http://myplugbox.com/oldcubox/  
Вот так было в первоначальном виде:
```
$ tree
.
├── README.md
├── boot
│   ├── boot.scr
│   ├── boot.scr.hdmi
│   ├── boot.scr.serial
│   ├── burn-u-boot-and-start-installer-serial.txt
│   ├── burn-u-boot-and-start-installer.txt
│   ├── images
│   │   ├── boot.scr
│   │   ├── modules.tar.xz
│   │   ├── rootfs.tar.xz
│   │   ├── rootfs.xbmc.trial.tar.xz
│   │   ├── uImage
│   │   ├── ubuntu-mmc0p1.txt
│   │   └── ubuntu-mmc0p2.txt
│   ├── initrd.img
│   ├── install.sh
│   ├── u-boot.bin
│   └── uImage
├── images
│   └── ubuntu10.img.xz
├── u-boot-cubox_hynix_cubox_1GB_uart.bin
├── u-boot-cubox_hynix_cubox_2GB_uart.bin
└── u-boot-cubox_hynix_cubox_unified_spi.bin
```

-

# CuBox1

## Как оживить CuBox1

Если вы не в чем не уверены (все совсем плохо), то последовательность следующая:

1. На флешку с FAT32 записать папку boot со всем содержимым (я не клал boot.scr.hdmi)
2. Карта MicroSD - на нее будет установлена Ubuntu
3. Установить, если необходимо, драйвер для доступа к консольному порту (см. ссылку 2) и XMODEM  
    Для MacOS можно вместо sx использовать lsz:  
      brew install lrzsz
4. В CuBox воткнуть флешку в верхний(!) порт, SD-карту, сеть, консольный USB кабель (от любого Android Phone)
5. В терминале подключиться к CuBox и послать туда u-boot загрузчик, соответсвующий Вашем CuBox (1 или 2 Gb).  
  На разных ОС будет по разному, см. ссылку 2.  
  Для MacOS (имя устройства может быть чуть другим):  
  screen /dev/tty.usbserial-V3Rev01Lot001 115200  
  Ctrl+A  
  :  
  exec !! lsz -X /path/to/u-boot-cubox_hynix_cubox_1GB_uart.bin  
  Enter
6. Нажать кнопку reset на CuBox (щель над HDMI (под LAN) по центру)
7. Воткнуть питание
8. Отпустить кнопку reset

  Если все сделано правильно, то набраться терпения и ждать (последний checkpoint 122280). В конце получитe готовый CuBox c Ubuntu 10.

9. Выключаете (poweroff)
10. Вынимаете флешку (и удаляете с нее boot от греха)

11. Дополнительно  
  Но, возможно (так пишут в интернетах, у меня начало сразу и много), в консоль начнет сыпаться ошибка вида (ее видно непосредственно в консоли, ну или в логах):  
  asoc: CS42L51 <-> mv88fx-i2s1 No matching rates  
  Раньше помогало:  
  sudo apt-get --reinstall install dbus  
  Я лечил вручную:  
  wget http://launchpadlibrarian.net/62421733/dbus_1.2.16-2ubuntu4.1_armel.deb (4.3 не нашел)  
  sudo dpkg -i dbus_1.2.16-2ubuntu4.1_armel.deb  
  И лечить лучше не с консоли, а зайдя по ssh, чтобы сообщения об ошибках не мешали.

### Если не надо переливать загрузчик, то почти тоже самое, кроме:
5 Не надо запускать заливку (для MacOS от нажимать Ctrl+A и далее)  
6 Не надо  
8 Не надо  
В целом можно даже не подключаться, а просто подождать минут 10, но это как-то экстримально)  
Можно попробовать посмотреть сразу все по HDMI, но мой CuBox был в состоянии, когда по HDMI он ничего не показывал. Поэтому я не знаю, как это будет.

### Полезные ссылки
1. http://sr.maltegrosse.de/index.php?title=CuBox_boot_process
2. http://sr.maltegrosse.de/index.php?title=Serial_console
3. https://www.youtube.com/watch?v=-gBoELdllnY
4. https://www.youtube.com/watch?v=kdP_au5if_k
5. http://forum.solid-run.com/miscellaneous-f26/cubox-1st-generation-t2209.html - отсюда были взяты все файлы

u-boot, который выложен тут, поддерживает ext4-fs. На тот случай, если вы захотите вместо 10-ой Ubuntu поставить что-то посвежее.

## Готовые образы
В папке images находится готовый образ получившийся из пунктов, описанных выше. Основной раздел на флешке (из этого образа) имеет ext4-fs, поэтому "родной" загрузчик, который поставлялся в оригинальном CuBox его не поймет (сам я по понятным причинам этого не проверял, но из того, что прочитал за последние сутки, вывод сделал).
