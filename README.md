# Название выполняемого задания;
Работа с загрузчиком

# Текст задания
Научиться попадать в систему без пароля;
Устанавливать систему с LVM и переименовывать в VG.

# Исходные данные
Гостевая ОС Ubuntu 24.04

## Включение отображения Grub меню

vagrant@belousovGrub:~$ sudo nano /etc/default/grub

Вносим изменения в файл
#GRUB_TIMEOUT_STYLE=hidden
GRUB_TIMEOUT=10

vagrant@belousovGrub:~$ sudo update-grub
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-6.8.0-31-generic
Found initrd image: /boot/initrd.img-6.8.0-31-generic
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
done

vagrant@belousovGrub:~$ sudo reboot


## Попасть в систему без пароля несколькими способами.

Вариант 1.
При перезапуске появляется меню GRUB, нажимаем "e"

Вот это...
linux	/vmlinuz-6.8.0-31-generic root=/dev/mapper/ubuntu--\
vg-ubuntu--lv ro net.ifnames=0 biosdevname=0 autoinstall ds=nocloud-net\
;s=http://10.0.2.2:8957/ubuntu/

Меняю на это...
linux	/vmlinuz-6.8.0-31-generic root=/dev/mapper/ubuntu--\
vg-ubuntu--lv rw init=/bin/bash 

Нажимаем F10

Вариант 2.
В меню GRUB

Выбираю Recovery mode
И все у меня root (как то немного отличается от методички возможно потому что у нас разные версии GRUB)

You are in rescue mode. After logging in, type system logs, "systemctl reboot" to reboot, or 1 to continue bootup.
Press Enter for maintenance or press Control-D to continue):

Нажимаем Enter

## Установить систему с LVM, после чего переименовать VG.

rootObelousovGrubi/home/vagrant# vgs
 VG	#PV #LV #SN Attr VSize VFree
 ubuntu-vg 110 wz--n- <62.09g 31.00g

root@belousovGrub :/home/vagrant# vgrename ubuntu-vg VG
 Volume group "ubuntu-vg" successfully renamed to "VG" init

 В файле /boot/grub/grub.cfg везде "ubuntu--vg" заменяем на "VG"

Перезапускаем 
 
Молимся

Проверяем

vagrant@belousovGrub:~$ sudo vgs
 VG #PV #LV #SN flt-tr VSize VFree
 VG 1	1	0 wz--n- <62.00g 31.00g
 