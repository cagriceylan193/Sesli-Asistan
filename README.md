
import speech_recognition as sr
from datetime import datetime
import webbrowser
import time
from gtts import gTTS, tts
from playsound import playsound
import random
import os
import sqlite3
import pandas as pd
import numpy as np
import seaborn as sb

from pyttsx3 import speak

r = sr.Recognizer()

#burdaki kelimeyi iki kere söylediğinizde yazılım kendin kapatıcak
badwords = pd.DataFrame({"badwords": ["mal"],
                         "used": [0]})


def record(ask=False):
    with sr.Microphone() as source:
        if ask:
            speak(ask)
        audio = r.listen(source)
        print(audio)
        voice = ''

        try:
            voice = r.recognize_google(audio, language="tr")

        except sr.UnknownValueError:
            speak1('Anlayamadım')
        except sr.RequestError:
            speak1('Sistem Çalışmıyor')
        return voice

#bu fonksiyona istediğiniz komutu verebilirsiniz
def response(voice):
    clock12 = time.strftime('%H')
    minute12 = time.strftime('%M')
    clock = datetime.now().strftime('%H:%M')
    tarih = time.strftime('%d:%B:%H:%M')
    year = time.strftime('%Y')
    day_name = time.strftime('%A')
    
        #Bu aralar yaygın olan canlı ders programınız için kullanabilirsiniz
    if "bugün canlı dersim var mı" in voice:
        if day_name == "Monday":
            speak1()
        if day_name == "Tuesday":
            speak1()
        if day_name == "Wednesday":
            speak1()
        if day_name == "Thursday":
            speak1()
        if day_name == "Friday":
            speak1()
        if day_name == "Saturday":
            speak1()
        if day_name == "Sunday":
            speak1()
    #günlük programınızı yazabilrisiniz
    if "bugünün programında ne var" in voice:
        speak1(
           
        )
        #biraz da eğlenmek için komutlar ekleyelim
    if "özür dilerim" in voice or "Özür dilerim" in voice:
        speak1("Tamam önemli değil ama Bir DAHA OLMASIN")

    if 'Neyi anladın ki zaten' in voice:
        speak1('Düzgün Söylersen Anlarım')
    if 'cevap ver' in voice:
        speak1('Pardon Uykum Geldi de')
    if 'teşekkür ederim' in voice:
        #enspeak fonksiyonu ile asistanınızı ingilizce konuşturabilirsiniz.
        enspeak('You Are Welcome Sir')
    if 'Ben de iyiyim sağ ol' in voice:
        speak1('Herzaman iyisin sen zaten başkanım')
    if 'günaydın' in voice:
        enspeak('goodmorning sir')
        speak1('tarih' + tarih)
      
    if 'Günaydın' in voice:
        enspeak('goodmorning sir')
        speak1('tarih' + tarih)
        
       
    if 'adım ne' in voice:
    #bu kısıma adınızı string formatında eklersiniz ya da bir değişkene atıp okutursunuz size kalmış
        speak1('Adının' 'Olduğu Yönünde Bilgi Alıyorum')
    if 'adın ne' in voice:
        enspeak('Jarvis')
    if 'nasılsın' in voice:
        speak1('İyiyim Sen Nasılsın')
    if 'spotifyı aç' in voice:
        os.system('Spotify.exe')
    if 'Spotify aç' in voice:
        os.system('Spotify.exe'),
        #Netflixten Film izleyebilisiniz
    if 'film izle' in voice:
        search = record(speak1('Hangi filmi açıyım '))
        url = 'https://www.netflix.com/browse?q=' + search
        webbrowser.get().open(url)
        speak1(search + "Senin İçin Bulduklarım")
        #buradaki url kısmına eokul ya da obs linkini verebilirsiniz
    if "notlarımı göster" in voice:
        url = ""
        webbrowser.get().open(url)
    if 'netflix aç' in voice:
        speak1('Netflix açılıyor')
        time.sleep(2)
        os.system('netflix')
        
    if "en sevdiğim müziği aç" in voice or "En Sevdiğim şarkıyı aç" in voice:
    #buradaki url ye en sevdiğiniz şarkının linkini verebilirsiniz
        url = ""
        webbrowser.get().open(url)

    if 'saat kaç' in voice:
        speak1(datetime.now().strftime('%H:%M'))
    if 'arama yap' in voice:
        search = record(speak1('Ne Aramak İstiyorsun'))
        url = 'https://google.com/search?q=' + search
        webbrowser.get().open(url)
        speak1(search + "  Senin İçin Bulduklarım")
    if 'müzik aç' in voice:
        search = record(speak1('Hangi Müziği İstiyorsunuz'))
        url = 'https://www.youtube.com/search?q=' + search
        webbrowser.get().open(url)
        speak1(search + "Keyfini Çıkar")
    if 'Tamam' in voice:
        enspeak("See You Later Boss")

        exit()
    if 'tamam' in voice:
    #yazılım tamam sesini algıladığında kapanıcaktır
        enspeak("See You Later Boss")
        exit()
    
#burada işin duygu kısmı ortaya çıkıyor eğer iki defa badwords dataframeindeki kelimeyi söylerseniz yazılım kendini kapatıcaktır 
    if badwords["used"].sum() / len(badwords["badwords"]) > 0:
        speak1("Böyle Konuşma Demiştim Dimi")
        exit()

    for i in badwords["badwords"]:
        if i in voice:
            speak1("Bidaha Benimle Böyle Konuşursan küserim")
            badwords["used"] = badwords["used"] + 1


def speak1(string):
    tts = gTTS(string, lang='tr')
    rand = random.randint(1, 10000)
    file = 'audio-' + str(rand) + '.mp3'
    tts.save(file)
    playsound(file)
    os.remove(file)


def enspeak(string1):
    tts1 = gTTS(string1, lang='en')
    rand = random.randint(1, 10000)
    file1 = 'audioeng-' + str(rand) + '.mp3'
    tts1.save(file1)
    playsound(file1)
    os.remove(file1)


speak1('Jarvis hizmetinizde')

while 1:
    voice = record()

    print(voice)
    response(voice)

