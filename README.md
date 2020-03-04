# shift_register 74hc595
**74hc595 shift register vezérlése MicroPythonnal**

Ezt a MicroPython modult a 74hc595 vezérlésére írtam. Segítségével három vezérlő lábról könnyedén kapcsolgathatunk sok vezérelt lábat (azért ha túl sok shift register-t kötünk sorba a vezetékek antennaként zajt kelthetnek, ilyenkor a shift regiszter "csodákra képes"). A modult különböző mcu-on próbáltam, teszteltem, amit az alábbiakban részletesen bemutatok.

> esp8266-01 modulon

Első lépésként a firmware-t kell, ráírni. Ennek a részletes leírást megtaláljátok egy előző munkámban: https://github.com/arnoldrobert/Flash-ESP-01-firmware . Ha rajta a MicroPython csatlakoztassuk le a GPIO0-t a GND(-)-ról és kezdőthet a játék.
A PuTTy-tól talán kényelmessebb a ***"uPyCraft IDE"*** alkalmazás, ami letölthető a következő hivatkozásról: https://randomnerdtutorials.com/uPyCraftWindows . A uPyCraft-ról itt olvashattok bővebben: https://dfrobot.gitbooks.io/upycraft/content/chapter1/jie-mian-ji-cai-dan.html . A uPyCraft indításakor kérni fogja, hogy telepítsük a SourceCodePro.ttf -et. Ezt a jobb gomb és **"Install for all users"** paranccsal tegyük, mert különben, minden alkalommal az alkalmazás indításkor kérni fogja, hogy telepítsük, nagyon idegesítő (lehet, hogy az újabb verzióknál már kijavították ezt a bug-ot).
A program megnyitása után a Tools legördülő menüben válasszuk ki a COM (nekem COM5) portunkat és az ESP8266 modult. Ha  sikerült a soros kommunikáció, akkor alul megjelenik a Python főprompt (>>>). Kipróbálhatunk egy print('go baby go')-t kiírni. Ezután be kell állítani, hogy a az ESP8266 tápra csatlakoztatáskor alapértelmezetten indítsa a WebREPL-t. Azért van erre szükség, mert az ESP-01-nek csak kettő szabadon használható kimenő/bemenő lába van (GPIO0 és GPIO2), és mivel a shift register vezérlésére három kimenő lábra van szükség a Tx-t is használjuk, vagyis a MicroPython **s.py** modulom feltöltése után, a soros kommunikációnak búcsút inthetünk. Ezután csak Wifi-n keresztül vezérelhetjük. Fontos a sorrend, először a WebREPL-t konfiguráljuk és csak utána töltjük fel az s.py filet, majd állítsuk alapértelmezettnek inuláskor. A WebREPL-t a következő hivatkozáson találjuk: https://github.com/micropython/webrepl . Androidos eszközre is telepíthető a mit a file-ok között **micropython.apk** néven találtok. Ha lementettük, egy "webrepl-master" mappában találjuk a webrepl.html file-t. Fontos a műveletek sorrendje, először a WebREPL-t kell configurálni és csak utána ráírni az s.py MicroPython modult.

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
2) Ha helyi hálózaton keresztül szeretnén elérni az ESP8266-ot
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

Az **"s.py"** file a repozitomból letölthető. A uPyCraft-ban miután rácsatlakoztunk a COM portunkra, lépkedjünk a File-Open-re, és nyissuk meg az s.py file-t. A uPyCraft jobb oldalán kattintsunk a DowenloadAndRun (>) gombra. Ha nem megy próbáljuk ki-be kapcsolni az ESP-t és újracsatlakoztatni a portra. Amikor feltöltötte, a uPyCraft bal felén, a device-ban megjelenik az s.py file (ha nem jelent meg akkor ki kell lépni a programból majd visszalépni). A uPyCraft bal oldalán a device-ben kattintsunk rá a boot.py file-ra és írjuk be import s (vagy valamelyik már meglévő import parancs után vesszővel elválasztva írjuk oda az s-t). Ezzel beállítottük, hogy tápra kapcsolva az ESP alapértelmezettként indítsa az s.py modult.
