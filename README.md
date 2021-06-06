Empezamos añadiendo tres librerías explicadas anteriormente:
```
#include "Audio.h"
#include "SD.h"
#include "FS.h"
```
Seguimos el programa con la asignación de los diferentes pines que vamos a utilizar. Los pines SPI_MOSI , SPI_MISO , SPI_SCK y SPI_CS son los de la tarjeta SD y los pines I2S_DOUT , I2S_BCLK y I2S_LRC los vamos a conectar al MAX98357A.
```
#define SD_CS 5
#define SPI_MOSI 23
#define SPI_MISO 19
#define SPI_SCK 18
#define I2S_DOUT 25
#define I2S_BCLK 27
#define I2S_LRC 26
```
Añadimos un objeto de Audio donde estableceremos todas las configuraciones. Siguiendo con la programación del void setup(), establecemos una comunicación serie a 11500 bauds, después con la función pinMode() establecemos SD_CS como salida y con digitalWrite() le asignamos el valor HIGH. A continuación los pines SPI_SCK, SPI_MISO y SPI_MOSI los estableceremos de tal manera que puedan establecer una recepción de daots del SD a la función SPI.begin(). En la siguiente línea con SD.begin() inicializa la biblioteca y la tarjeta SD para el pni SD_CS. Seguimos con la asignacion de los pines de salida del dispositivo I2S, y en la siguiente línea configuramos el volumen con audio.setVolume(15) (utilizamos 15 por ser una cifra intermedia entre 0 y 21, ya que el rango es ese). Por ultimo, teniendo en cuenta que el fichero lee de la SD, utilizaremos la función audio.connecttoFS() la cual recibe los siguientes valores:
- El objeto SD
- Nombre del fichero que vamos a reproducir
```
void setup(){
Serial.begin(115200);
pinMode(SD_CS, OUTPUT);
digitalWrite(SD_CS, HIGH);
SPI.begin(SPI_SCK, SPI_MISO, SPI_MOSI);
SD.begin(SD_CS);
audio.setPinout(I2S_BCLK, I2S_LRC, I2S_DOUT);
audio.setVolume(15);
audio.connecttoFS(SD, "Ensoniq-ZR-76-01-Dope-77.wav");
}
```
Por ultimo, añadimos en el void loop() la función audio.loop(), de esta manera, reproduciremos continuamente el audio
```
void loop(){
audio.loop();
}
```