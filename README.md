# Caja de 37 sensores
### Sensores utilizados: Sensor RGB, Sensor laser, y el sensor Photosensible.

### Sistemas programables
### Instituto tecnologico de tijuana
- Integrantes del equipo
- Castro Pacheco Jose Manuel
- Benitez Peraza Joshua
- Cruz Eduardo Valadez Melendez
- Garcia Juarez Isai

### Docente
-Rene Solis Reyes

En la siguiente imagen se logra apreciar los componentes del proyecto.

<img src="sensor.JPG" alt="sensor" width="500" height="600">

Ahora se mostrara dicho codigo utilizado para hacer el programa:
#Joshua Benitez Peraza
#Castro Pacheco Jose
#El programa requiere y hace uso de los siguientes sensores: Photosensible, RGB, Laser.
#El codigo siguiente permite hacer uso de los sensores ya mencionados para hacer un detector del laser.
#Se prende el laser que se debera apuntar al sensor photosensible, de esta manera el led RGB pasara de estar
#parpadeando azul y rojo, a ver significando que esta recibiendo luz, adicionalmente los parametros,
#obtenidos por los sensores se estaran imprimiendo en consola.


from machine import Pin,ADC,PWM
from time import sleep
 
photo = 0               # ADC0 multiplexing pin is GP26
LaserPin = 5           #Red laser transmitter
Led_R = PWM(Pin(2))
Led_G = PWM(Pin(3))
Led_B = PWM(Pin(4))
Led_R.freq(2000)   
Led_G.freq(2000)   
Led_B.freq(2000)   
 
def setup():
    global Laser
    global photo_ADC
    photo_ADC = ADC(photo) 
    Laser = Pin(LaserPin,machine.Pin.OUT) 
    Laser.value(0) 
 
def loop():
    aim_time = 0
    while True:  
        Laser.value(1) 
        Led_R.duty_u16(0) 
        Led_G.duty_u16(65535)
        Led_B.duty_u16(0)
        
        print("time:",aim_time)
        sleep(0.1)
        aim_time += 0.1
        if photo_ADC.read_u16() < 1035:     # Hit by a red laser, the light flashes
            print("Aimed:",aim_time)
            aim_time = 0
            for i in range(10):
                Led_R.duty_u16(65535)       # red light
                Led_G.duty_u16(0)
                Led_B.duty_u16(0) 
                sleep(0.1)  
                Led_R.duty_u16(0)           
                Led_G.duty_u16(0)
                Led_B.duty_u16(65535)       # blue light
                sleep(0.1)
            sleep(2)
 
if __name__ == '__main__':
    setup() 
    loop() 
