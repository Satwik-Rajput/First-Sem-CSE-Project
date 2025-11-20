# Voice-Activated Virtual Assistant in Python
README: Voice-Activated Virtual Assistant ("Jarvis")

# Problem-Statement
Design and implement a Python-based voice-activated virtual assistant, "Jarvis", which listens for a wake word, recognizes speech commands, accesses web resources (such as news, Google, YouTube, LinkedIn, and GitHub), reads news aloud, and utilizes AI to process general questions with context-aware responses. The assistant must use speech recognition, text-to-speech, web API requests, and generative AI models, demonstrating integration of multiple Python modules for robust, real-time user interaction.

# Theory and Concepts

This project combines the following technologies:

Speech Recognition: Converts spoken language into text, allowing hands-free interaction.

Text-to-Speech (TTS): Enables Jarvis to respond vocally, providing auditory feedback.

Web API Integration: Fetches dynamic information (news) from web sources using APIs.

Web Automation: Opens various websites upon command.

Generative AI: Responds to non-predefined queries using Google's generative AI (Gemini).

| Module                  | Purpose                                                  |
| ----------------------- | -------------------------------------------------------- |
| speech_recognition (sr) | Recognizes and converts spoken words to text.            |
| pyttsx3                 | Synthesizes spoken responses based on text input.        |
| webbrowser              | Opens web URLs in the default system browser.            |
| requests                | Makes HTTP requests for fetching web data (news API).    |
| google.genai            | Connects to Google Gemini API for generative AI answers. |

#You can install required modules with:
pip install SpeechRecognition pyttsx3 requests google-generativeai

You may need to separately arrange credentials and API keys for newsapi.org and Google Gemini (via Google Cloud Console).

# Code Breakdown and Major Statements

# 1. Initialization
The assistant initializes the speech recognizer, text-to-speech engine, and API keys.

    import speech_recognition as sr
    import pyttsx3
    import webbrowser
    import requests
    from google import genai

    recognizer = sr.Recognizer()
    engine = pyttsx3.init()
    newsapi = "YOUR_NEWSAPI_KEY"

Purpose: Prepares all modules and keys, making the assistant ready for listening and responding.

# 2. Text-to-Speech Function
Converts text responses to audible speech.

    python
    def speak(text):
    engine.say(text)
    engine.runAndWait()
Usage: Called whenever Jarvis needs to vocalize feedback or information.

# 3. AI Processing via Gemini
Handles generic, non-predefined queries using generative AI.

    python
    def aiprocess(command):
    client = genai.Client(api_key="YOUR_GEMINI_API_KEY")
    response = client.models.generate_content(
        model="gemini-2.5-flash",
        contents=[
            "You are a virtual assistant named jarvis skilled in general tasks...",
            command
        ],
    )
    return response.text
Usage: Delegates open-ended questions to Google Gemini and returns concise AI-generated answers.

# 4. Command Processing Logic
Decides action based on keywords found in recognized text commands.

    python
    def processcommand(c):
    # Opens website if recognized phrase
    if "open google" in c.lower():
        webbrowser.open("https://www.google.com")
    ...
    # Fetches news if 'news' is mentioned
    elif "news" in c.lower():
        r = requests.get(f"https://newsapi.org/v2/top-headlines?country=us&apiKey={newsapi}")
        if r.status_code == 200:
            data = r.json()
            articles = data.get('articles', [])
            for article in articles:
                print(article['title'])
                speak(article['title'])
    else:
        # Use AI for other queries
        output = aiprocess(c)
        print(output)
        speak(output)
Purpose: Matches user intentions with functions like browsing, news fetching, or AI Q&A.

# 5. Main Loop and Voice Interaction
Continuously listens for the wake word ("Jarvis"), activates upon recognition, processes the next command, and responds.

    python
    if __name__ == "__main__":
    speak("Initializing jarvis...")
    while True:
        r = sr.Recognizer()
        print("recognizing...")

        try:
            with sr.Microphone() as source:
                r.adjust_for_ambient_noise(source)
                print("Listening...")
                audio = r.listen(source)
            word = r.recognize_google(audio)

            if word.lower() == "jarvis":
                speak("welcome sir")
                # Listen for command
                with sr.Microphone() as source:
                    r.adjust_for_ambient_noise(source)
                    print("Jarvis active...")
                    audio = r.listen(source)
                    command = r.recognize_google(audio)
                    processcommand(command)

        except Exception as e:
            print("Error;{0}".format(e))
Usage: Ensures hands-free activation and query processing pipeline.

# Demonstration & Usage
Run the script and allow microphone access.

Say "Jarvis" to activate the assistant.

Given commands like:

"Open Google"

"News"

"What's today's weather?"

"Who is the prime minister of India?"

Assistant will respond via voice and, if required, display relevant text/actions (i.e., open web links, read out news, or answer generative questions).

# Conclusion
This project demonstrates voice-driven automation and intelligent Q&A using a combination of speech recognition, web APIs, and generative AI. It shows how Python can integrate hardware input (microphone), cloud AI, and real-time web resources for a natural interaction experience in virtual assistant projects.

# References:
SpeechRecognition documentation, pyttsx3 documentation, NewsAPI docs
Google Generative AI (Gemini), Python module docs

Feel free to adjust the API keys and customize more code comments for your submission requirements.

Unlock web app generation
