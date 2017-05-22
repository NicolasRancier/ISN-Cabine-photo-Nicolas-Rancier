# ISN-Cabine-photo-Nicolas-Rancier
#Déclarer les variables
from picamera import PiCamera 
from time import sleep
import time
from grovepi import *
import subprocess
import os

#Espace restant ou non pour prendre la photo

stat = os.statvfs('cabine photo.py')
espace_min = 2500000
espace_restant = stat.f_frsize * stat.f_bfree 
#numéro du port sur lequel est branché la LED
led = 4
#LED éteinte
switch = 0

pinMode(led, "OUTPUT")

camera = PiCamera()
#capture de la photo tant qu'il y a de l'espace libre lors de #l'execution du programme
camera.resolution = (2592, 1944)
camera.start_preview()
while True:
  if espace_restant < espace_min:
    print('plus de place')
    break
  for i in range(11):
    camera.annotate_text = "Souhaitez vous conserver cette photo ?"
    camera.annotate_text_size = 50
    switch = not switch
    digitalWrite(led, switch) #LED clignote sur une intervalle de 1s
    sleep(1)
    print(10-1)
    if i==10:
      print('Souriez !!')
#image enregistrée sur le bureau
   camera.capture('/home/pi/Desktop/image0.jpg')
   subprocess.call(["gpicview","/home/pi/Desktop/image0.jpg"])
   camera.stop_preview()

