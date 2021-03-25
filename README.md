# Led Control with Voice Command
LED control with voice command with Raspberry Pi

#!/usr/bin/python3 
# - *- coding: utf- 8 - *-

import speech_recognition as sr 
import RPi.GPIO as GPIO   
import os  
import time     

GPIO.setwarnings(False)   #tunr off warnings
GPIO.setmode(GPIO.BOARD) #using board pin not GPIOxx
GPIO.setup(11, GPIO.OUT)     #set output 11. board pin (GPIO17)

r = sr.Recognizer()       
speech = sr.Microphone(device_index=1)  #our mic's index

def mic_text_doc():                        
    recog = "-"   
    with speech as source:
        print("Dinliyorum")
        r.adjust_for_ambient_noise(source)  
        audio = r.listen(source) 
        try:
            recog = r.recognize_google(audio, language = 'TR- tr') #setup your language (like EN-en)
        except sr.UnknownValueError:                  
            print("Anlaşılmadı")
        except sr.RequestError as e:
            print("Servis hatası; {0}".format(e))
    
    print("Bunu dedin: " + recog)   
    return recog

def led_on(pin): #led on fucntion
    os.system("mpg321 /home/pi/Downloads/led/led_on.mp3  &") #don't forget to edit file path
    time.sleep(0.5) #delay 
    GPIO.output(pin,GPIO.HIGH)  #HIGH (3.3V)
    time.sleep(0.5)
    

def led_off(pin): #led off function
    os.system("mpg321 /home/pi/Downloads/led/led_off.mp3 &") #don't forget to edit file path
    time.sleep(0.5)
    GPIO.output(pin,GPIO.LOW) #LOW (0V)
    time.sleep(0.5)
    
while True: #infinity loop
     recog = mic_text_doc().lower() 
     text=recog
     
     if "aç" in text:  #if we said "ledi aç" or "aç" etc.
         led_on(11)  
            
     if "kapat" in text:
         led_off(11)  