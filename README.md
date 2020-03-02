# shift_register 74hc595
**74hc595 shift register vezérlése MicroPython-nal**

Ezt a MicroPython modult a 74hc595 vezérlésére írtam. Segítségével három vezérlő lábról könnyedén kapcsolgathatunk sok vezérelt lábat (azért ha túl sok shift register-t kötünk sorba a vezetékek antennaként zajt kelthetnek, ilyenkor a shift regiszter "csodákra képes"). A modult különböző mcu-on próbáltam, teszteltem, amit az alábbiakban részletesen bemutatok.

> esp8266-01 modulon

Első lépésként a firmware-t kell, ráírni. Ennek a részletes leírást megtaláljátok egy előző munkámban: https://github.com/arnoldrobert/Flash-ESP-01-firmware . Ha rajta a MicroPython csatlakoztassuk le a GPIO0-t a GND(-)-ról és kezdőthet a játék.
A PuTTy-tól talán kényelmessebb a ***"uPyCraft IDE"*** alkalmazás, ami letölthető a következő hivatkozásról: https://randomnerdtutorials.com/uPyCraftWindows . A uPyCraft-ról itt olvashattok bővebben: https://dfrobot.gitbooks.io/upycraft/content/chapter1/jie-mian-ji-cai-dan.html . A uPyCraft indításakor kérni fogja, hogy telepítsük a SourceCodePro.ttf -et. Ezt a jobb gomb és **"Install for all users"** paranccsal tegyük, mert különben, minden alkalommal az alkalmazás indításkor kérni fogja, hogy telepítsük, nagyon idegesítő (lehet, hogy az újabb verzióknál már kijavították ezt a bug-ot).
A program megnyitása után a Tools legördülő menüben válasszuk ki a COM (nekem COM5) portunkat és az ESP8266 modult. Ha  sikerült a soros kommunikáció, akkor alul megjelenik a Python főprompt (>>>). Kipróbálhatunk egy print('go baby go')-t kiírni. Ezután be kell állítani, hogy a az ESP8266 tápra csatlakoztatáskor alapértelmezetten indítsa a WebREPL-t. Azért van erre szükség, mert az ESP-01-nek csak kettő kimenő/bemenő lába van (GPIO0 és GPIO2), és mivel a shift register vezérlésére három lábra van szükség a Tx-t is használjuk, vagyis a MicroPython modulom feltöltése után a soros kommunikációnak búcsút inthetünk. Ezután csak Wifi-n keresztül vezérelhetjük. A WebREPL-t a következő hivatkozáson találjuk: https://github.com/micropython/webrepl . Ha lementettük, egy "webrepl-master" mappában találjuk a webrepl.html file-t. 

> WebREPL beállítása lépésről lépésre:

1) Ha közvetlenül az ESP8266-ra szeretnénk csatlakozni Wifi-n
```
- a uPyCraft prompt-ba írjuk a következőt,
    **import webrepl_setup**
- engedélyezzük (E + Enter),
- vidd be a jelszódat majd fogadd el (y + Enter),
- ezek után az internet elérések között meg kell, hogy jelenjen az ESP modulunk,
- csatlakozzunk rá az "micropythoN" jelszóval,
- nyissuk meg a webrepl.html -t és kapcsolódjunk (connect),
- írjuk be a megadott jelszónkat(nem látszódik),
- ha sikerült megjelenik a főpromt, próbáljuk ki egy print('go baby go') paranccsal, hogy működik-e,

```
2) Ha helyi hálózaton keresztül szeretnén elérni az ESP8266-ot
```
    import network
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    wlan.connect('ssid', 'password')
```

A shift register MicroPython modulomat **"s.py"** (a file-ok között található) be kell húzni a uPyCraft ablakba.
