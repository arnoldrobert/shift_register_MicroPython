# shift_register 74hc595
74hc595 shift register vezérlése MicroPython-al

Ezt a MicroPython modult a demultiplexer vezérlésére írtam. Segítségével könnyedén kapcsolgathatjuk a lábakat. A modul neve csak ' s.py ' , könnyedén importálható. Különböző mcu-on próbáltam, teszteltem, amit az alábbiakban részletesen bemutatok.
Mindenekelőtt telepíteni kell a Python 3 és az esptool alkalmazásokat (win10-re), amit a következő hivatkozásról lehet letölteni: [https://github.com/espressif/esptool](url). 

**esp8266-01 modulon**
Első lépésként a firmware-t kell, ráégetni. Ezekből a modulokból készítettek 512 KByte és 1MByte -os verziókat. Az 512 KByte-os számunkra nem kielégítő. A hardver követelményekről a MicroPython oldalán lehet informálódni: [http://docs.micropython.org/en/latest/esp8266/tutorial/intro.html](url). 
Az esp-t flash módba kell kötni, aminek a kapcsolási rajzát megtaláljátok a file-ok között 'esp flash mode.jpg' alatt. Ez a modul UART soros kommunikációt használ. Ügyelni kell az USB to serial modulnál arra, hogy a 3.3V-ra kössük, mert 5V-on elfüstöl az esp. Arduino-al is próbáltam, szintén csak a 3.3V-os tápfeszültségre kell ügyelni, valamint az Arduino Tx lábát feszültségosztó közbeiktatásával kell az esp Rx lábára kötni, mert az Arduino Tx lábán is 5V feszültség van, ami az esp_arduino_flash_mode.png file-ban megtekinthető.
Ha ezzel megvagyunk, akkor a cmd-be írjuk a következő parancsot: ' **esptool flash_id** ', ami megjeleníti az esp paramétereit. Mivel rá szeretnénk írni a MicroPython keret file-t, mindent törölnünk kell róla a következő cmd paranccsal: ' **esptool erase_flash** '. Ugyan ez megoldható a nodemcu-firmware 2.2.1 verzió segítségével is, amit itt találtok: [https://github.com/nodemcu/nodemcu-firmware/releases](url), mégpedig egy 1 MByte-os üres '**blank_1MB.bin**' file-t írunk rá, ami szinén megtalálsz a repozitori-ban.
