# shift_register 74hc595
**74hc595 shift register vezérlése MicroPythonnal**

Ezt a MicroPython modult a 74hc595 vezérlésére írtam. Segítségével három vezérlő lábról könnyedén kapcsolgathatunk sok vezérelt lábat (azért ha túl sok shift register-t kötünk sorba a vezetékek antennaként zajt kelthetnek, ilyenkor a shift regiszter "csodákra képes"). A modult különböző mcu-on próbáltam, teszteltem, amit az alábbiakban részletesen bemutatok.

> esp8266-01 modulon

Első lépésként a firmware-t kell, ráírni. Ennek a részletes leírását megtaláljátok egy előző munkámban: https://github.com/arnoldrobert/Flash-ESP-01-firmware . Ha rajta a MicroPython csatlakoztassuk le a GPIO0-t a GND(-)-ról és kezdődhet a játék.
A PuTTy-tól talán kényelmesebb a ***"uPyCraft IDE"*** alkalmazás, ami letölthető a következő hivatkozásról: https://randomnerdtutorials.com/uPyCraftWindows . A uPyCraft-ról itt olvashattok bővebben: https://dfrobot.gitbooks.io/upycraft/content/chapter1/jie-mian-ji-cai-dan.html . A uPyCraft indításakor kérni fogja, hogy telepítsük a SourceCodePro.ttf -et. Ezt a jobb gomb és **"Install for all users"** paranccsal tegyük, mert különben, minden alkalommal az alkalmazás indításkor kérni fogja, hogy telepítsük, nagyon idegesítő (lehet, hogy az újabb verzióknál már kijavították ezt a bug-ot).
A program megnyitása után a Tools legördülő menüben válasszuk ki a COM (nekem COM5) portunkat és az ESP8266 modult. Ha  sikerült a soros kommunikáció, akkor alul megjelenik a Python főprompt (>>>). Kipróbálhatunk egy print('go baby go')-t kiírni. Ezután be kell állítani, hogy a az ESP8266 tápra csatlakoztatáskor alapértelmezetten indítsa a WebREPL-t. Azért van erre szükség, mert az ESP-01-nek csak kettő szabadon használható kimenő/bemenő lába van (GPIO0 és GPIO2), és mivel a shift register vezérlésére három kimenő lábra van szükség a Tx-t is használjuk, vagyis a MicroPython **s.py** modulom feltöltése után, a soros kommunikációnak búcsút inthetünk. Ezután csak Wifi-n keresztül vezérelhetjük. Fontos a sorrend, először a WebREPL-t konfiguráljuk és csak utána töltjük fel az s.py filet, majd állítsuk alapértelmezettnek induláskor. A WebREPL-t a következő hivatkozáson találjuk: https://github.com/micropython/webrepl . Androidos eszközre is telepíthető amit a repozitban **micropython.apk** néven találtok. Ha lementettük, egy "webrepl-master" mappában találjuk a webrepl.html file-t.

> WebREPL beállítása lépésről lépésre:

1) Ha közvetlenül az ESP8266-ra szeretnénk csatlakozni Wifi-n
```
- a uPyCraft prompt-ba írjuk a következőt,
    import webrepl_setup
- engedélyezzük (E + Enter),
- vidd be a jelszódat majd fogadd el (y + Enter),
- a uPyCraft bal felén, a device-ban megjelenik a boot.py file
- ezek után a mobil eszközünkön, az internet elérések között, meg kell, hogy jelenjen az ESP modulunk,
- csatlakozzunk rá az "micropythoN" (alapértelmezett) jelszóval,
- nyissuk meg a webrepl.html -t és kapcsolódjunk (connect),
- írjuk be a megadott jelszónkat(nem látszódik),
- ha sikerült megjelenik a főpromt, próbáljuk ki egy print('go baby go') paranccsal, hogy működik-e,

```
2) Ha helyi hálózaton keresztül szeretnénk elérni az ESP8266-ot
```
- a uPyCraft prompt-ba írjuk egyenként a következőket,
    import network
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    wlan.connect('ssid', 'password') # a helyi hálózatunk neve és jelszava
    wlan.ifconfig()
- az első kapott IP-n tudunk csatlakozni majd,
- cseréljük ki a WebREPL alapértelmezett IP-ét a kiosztott IP-re és kapcsolódjunk.
```
> A MicroPython "s.py" modulom ráírása az ESP-re

Lábkiosztás:
```
ESP-01        Shift Register
 GPIO0         SER(adat)
 GPIO2         RCLK(zar)
 GPIO1         SRCLK(orajel)
```
A file-ok közé feltöltöttem az ESP-01 lábkiosztását esp8266_pinout.jpg néven. A különböző mikrovezérlők lábkiosztásai eltérőek, ezért a s.py-MicroPython modulom a kódjában ebben eltér egymástól. A nevét viszont meghagytam mindegyiknél egyformára, hogy rövidebb legyen meghívni a file-ba. Az **"s.py"** file a repozitomból letölthető, csak ügyelj, hogy a megfelelő mappát válaszd. A uPyCraft-ban miután rácsatlakoztunk a COM portunkra, lépkedjünk a File-Open-re, és nyissuk meg az s.py file-t. A uPyCraft jobb oldalán kattintsunk a DowenloadAndRun (>) gombra. Ha nem megy próbáljuk ki-be kapcsolni az ESP-t és újra csatlakoztatni a portra. Amikor feltöltötte, a uPyCraft bal felén, a device-ban megjelenik az s.py file (ha nem jelent meg akkor ki kell lépni a programból majd visszalépni). A uPyCraft bal oldalán a device-ben kattintsunk rá a boot.py file-ra és írjuk be **"import s"** (vagy valamelyik már meglévő import parancs után vesszővel elválasztva írjuk oda az s-t). Ezzel beállítottuk, hogy tápra kapcsolva az ESP alapértelmezettként indítsa az s.py modult.

> NodeMCU - esp8266-12 modulon

Gyakorlatilag minden megegyezik az ESP-01 -nél leírtakkal, azzal a különbséggel, hogy USB és bootloader segítségével programozzuk, valamint a lábkiosztás is változik a következők szerint:
```
NodeMCU       Shift Register
 D5(Pin14)       SER(adat)
 D6(Pin12)       RCLK(zar)
 D7(Pin13)       SRCLK(orajel)
```

Az s.py modul részletes bemutatását a következő oldalon lehet elérni: https://github.com/arnoldrobert/s.py_modul_bemutatasa .
 
