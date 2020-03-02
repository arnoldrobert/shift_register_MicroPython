# shift_register 74hc595
**74hc595 shift register vezérlése MicroPython-nal**

Ezt a MicroPython modult a 74hc595 vezérlésére írtam. Segítségével három vezérlő lábról könnyedén kapcsolgathatunk sok vezérelt lábat (azért ha túl sok shift register-t kötünk sorba a vezetékek antennaként zajt kelthetnek, ilyenkor a shift regiszter "csodákra képes"). A modult különböző mcu-on próbáltam, teszteltem, amit az alábbiakban részletesen bemutatok.

> esp8266-01 modulon

Első lépésként a firmware-t kell, ráírni. A részletes leírást megtaláljátok itt: https://github.com/arnoldrobert/Flash-ESP-01-firmware . Ha rajta a MicroPython kezdőthet a játék.
A PuTTy-tól talán kényelmessebb a ***"uPyCraft IDE"*** alkalmazás, ami letölthető a következő hivatkozásról: https://randomnerdtutorials.com/uPyCraftWindows . 
A uPyCraft indításakor kérni fogja, hogy telepítsük a SourceCodePro.ttf -et. Ezt a jobb gomb és **"Install for all users"** paranccsal tegyük, mert különben, minden alkalommal az alkalmazás indításkor kérni fogja, hogy telepítsük, nagyon idegesítő (lehet, hogy az újabb verzióknál már kijavították ezt a bug-ot).
