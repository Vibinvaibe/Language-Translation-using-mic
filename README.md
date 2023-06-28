# Language-Translation-using-mic

This code is a simple Python script that demonstrates the conversion of speech to text using the SpeechRecognition library and translates the recognized text into another language using the Google Translate API. The translated text is then converted back into speech using the gTTS (Google Text-to-Speech) library.

# Dependencies
Before running this code, make sure you have the following dependencies installed:

SpeechRecognition (pip install SpeechRecognition)
langdetect (pip install langdetect)
iso639 (pip install iso639)
googletrans (pip install googletrans)
pyttsx3 (pip install pyttsx3)
pygame (pip install pygame)
gTTS (pip install gtts)

# Usage
Import the necessary libraries
```
import speech_recognition as s_r
from langdetect import detect
from langdetect import detect_langs
from iso639 import languages
from langdetect import DetectorFactory
DetectorFactory.seed = 0
from googletrans import Translator
import pyttsx3
from pygame import mixer
from gtts import gTTS
```
Define the 'speak' function, which converts text into speech and plays the audio:
```
def speak(data, dest_lang):
    global count
    filename = f'speech{count % 2}.mp3'
    tts = gTTS(text=data, lang=dest_lang)
    tts.save(filename)
    mixer.init()
    mixer.music.load(filename)
    mixer.music.play()
    count += 1
```
Initialize the SpeechRecognition recognizer and microphone:
```
r = s_r.Recognizer()
my_mic = s_r.Microphone()
```
Define the listen function, which records audio from the microphone and converts it to text using Google's speech recognition service:
```
def listen():
    with my_mic as source:
        print("Say now!!!!")
        r.adjust_for_ambient_noise(source)  # reduce noise
        audio = r.listen(source)  # take voice input from the microphone
    sentence = r.recognize_google(audio)  # convert voice into text
    print(sentence)
    return sentence
```
Create an instance of the Translator class for translating text:
```
translator = Translator()
```
Define the translate function, which detects the source language (if not provided), determines the language code for the destination language, and performs the translation using the Google Translate API:
```
def translate(sentence, dest_lang, src_lang=None):
    # src lang
    if src_lang == None:
        src_lang = detect(sentence)
    # dest lang
    for i in languages:
        if dest_lang == i.name:
            dest_lang_code = i.part1
    # translation
    result = translator.translate(sentence, src=src_lang, dest=dest_lang)
    print(result)
    return result, dest_lang_code
```
Call the listen function to record audio and convert it into text:
```
sentence = listen()
```
Call the translate function to translate the text into the desired language (e.g., French) and obtain the translation and destination language code:
```
result, dest_lang_code = translate(sentence, 'French', 'English')
```
Use the speak function to convert the translated text into speech and play it:
```
speak(result.text, dest_lang_code)
```
# Notes
This code assumes that you have an active internet connection to access the Google Translate API.
The audio playback for the converted speech requires the pygame library.
The code saves the generated speech as an MP3 file (speech{count % 2}.mp3) and plays it. You can modify the code to change the file format or adjust the audio playback settings as needed.
The destination language should be provided as a string. You can refer to the languages list from the iso639 library to get the correct language names.

# Disclaimer
This code is provided as a basic example and may require additional error handling, input validation, or modifications for production use. Use it at your own risk.
