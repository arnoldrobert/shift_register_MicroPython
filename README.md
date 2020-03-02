# shift_register 74hc595
**74hc595 shift register vezérlése MicroPython-al**

Ezt a MicroPython modult a 74hc595 vezérlésére írtam. Segítségével három vezérlő lábról könnyedén kapcsolgathatunk sok vezérelt lábat (azért ha túl sok shift register-t kötünk sorba a vezetékek antennaként zajt kelthetnek, ilyenkor a shift regiszter "csodákra képes"). A modult különböző mcu-on próbáltam, teszteltem, amit az alábbiakban részletesen bemutatok.

> esp8266-01 modulon

Első lépésként a firmware-t kell, ráírni.
