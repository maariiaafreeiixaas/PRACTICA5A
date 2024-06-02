# Pràctica 5

## Practica 5A: Escàner I2C

### Codi

```cpp
#include <Arduino.h>
#include <Wire.h>

void setup() 
{ 
  Wire.begin(); 
  Serial.begin(115200); 
  while (!Serial);             // Leonardo: espera a que s'iniciï el monitor sèrie 
  Serial.println("\nI2C Scanner"); 
}

void loop() 
{ 
  byte error, address;  
  int nDevices; 
  Serial.println("Scanning..."); 
  nDevices = 0; 
  for(address = 1; address < 127; address++ ) 
  { 
    // L'escàner I2C utilitza el valor de retorn de
    // Wire.endTransmission per veure si un dispositiu
    // va reconèixer l'adreça.
    Wire.beginTransmission(address); 
    error = Wire.endTransmission(); 
    if (error == 0) 
    { 
      Serial.print("I2C device found at address 0x"); 
      if (address < 16) 
        Serial.print("0"); 
      Serial.print(address, HEX); 
      Serial.println("  !"); 
      nDevices++; 
    } 
    else if (error == 4) 
    { 
      Serial.print("Unknown error at address 0x"); 
      if (address < 16) 
        Serial.print("0"); 
      Serial.println(address, HEX); 
    }     
  } 
  if (nDevices == 0) 
    Serial.println("No I2C devices found\n"); 
  else 
    Serial.println("done\n"); 
  delay(5000);           // espera 5 segons per al següent escaneig 
}
```

### Descriure la sortida pel port sèrie:

I2C Scanner
Scanning...
I2C device found at address 0x3C  !
I2C device found at address 0x68  !
done
...

### Funcionament
Aquest codi escaneja el bus I2C per trobar dispositius connectats i imprimeix les seves adreces pel port sèrie. 

Inicia el bus I2C i el port sèrie, després espera fins que el monitor sèrie estigui llest. 
Imprimeix un missatge indicant que l'escaneig ha començat i reinicia el comptador de dispositius trobats.
Un bucle escaneja les adreces de 1 a 126 per iniciar una transmissió a l'adreça actual i seguidament finalitzar la transmissió i captura el codi d'error.

Si error == 0, un dispositiu ha respost a l'adreça actual:
Imprimeix l'adreça del dispositiu I2C trobat.
Incrementa el comptador de dispositius trobats.
Si error == 4, es produeix un error desconegut:
Imprimeix un missatge indicant un error desconegut a l'adreça actual.

Si no es troba cap dispositiu (nDevices == 0), imprimeix "No I2C devices found".
Si es troben dispositius, imprimeix "done".
Espera 5 segons abans de repetir l'escaneig.