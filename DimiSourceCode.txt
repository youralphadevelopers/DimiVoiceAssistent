import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import webbrowser
import os
import smtplib
from random import randrange

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wishme():
    hour = int(datetime.datetime.now().hour)
    if 0 <= hour <= 12:
        speak("Good Morning Sir, this is Dimi, At your service.")
    elif 12 <= hour <= 18:
        speak("Good afternoon Sir, this is Dimi, At your service.")
    else:
        speak("Good evening Sir, this is Dimi, At your service.")


def takeCommand():
    # It takes microphone input from the user and returns string output

    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='en-in')
        print(f"User said: {query}\n")

    except Exception as e:
        # print(e)
        print("Say that again please...")
        return "None"
    return query


def sendEmail(param1, param2):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('YourEmail@gmail.com', 'YourPassword')
    server.sendmail('YourEmail@gmail.com', param1, param2)
    server.close()


if __name__ == '__main__':
    wishme()
    while True:
        query = takeCommand().lower()
        # Logic for executing tasks based on query

        if 'who is' in query:
            speak('Searching the web...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=1)
            speak("According to Web")
            print(results)
            speak(results)

        elif 'open youtube' in query:
            speak('Opening youtube for you')
            webbrowser.open("youtube.com")

        elif 'open google' in query:
            speak('Opening Google for you')
            webbrowser.open("google.com")

        elif 'open my instagram' in query:
            speak('Opening Instagram for you')
            webbrowser.open("instagram.com")

        elif 'who are you' in query:
            speak('I am Dimi... Your personal voice assistant made by Dev Mishra.')

        elif 'who made you' in query:
            speak('i was made by Dev Mishra.')

        elif 'more about yourself' in query:
            speak('I am Dimi... I was made by using python3.8, by Dev Mishra. Tell me what can i do for you')

        elif 'packages used' in query:
            speak('Sorry, I am not supoosed to tell you that.')

        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            print(strTime)
            speak(f"the time in is {strTime}")

        elif 'send email to' in query:
             try:
                speak("What should i say? ")
                content = takeCommand()
                to = "Receiver'sEmail@gmail.com"
                sendEmail(to, content)
                speak("Your Email has been sent!")
             except Exception as e:
                 print(e)
                 speak("Sorry unable to send email, please check you Network connection")

        elif 'play music' in query:
            music_dir = 'C:\\Users\\AlphaBoi\\Downloads\\Linkin Park - Greatest Hits [MP3~320Kbps]~[Hunter] [FRG]\\Linkin Park-Greatest Hits 320kbps\\CD1'
            songs = os.listdir(music_dir)
            print(songs)
            os.startfile(os.path.join(music_dir, songs[randrange(10)]))

        elif 'turn off' in query:
            speak('Turning off')
            quit()
