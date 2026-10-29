# Jarvis AI - Python Desktop Voice Assistant

Jarvis is a desktop voice assistant inspired by Iron Man, written in Python. It listens to your voice through the microphone, understands what you say, and replies by speaking back. It can search Wikipedia, open websites, play music, tell the time, open VS Code and send emails.


 
## Features

- Greets you with good morning, good afternoon or good evening based on your system time
- Voice recognition using Google Speech Recognition
- Text-to-speech replies using `pyttsx3`
- Wikipedia search with a spoken summary
- Open YouTube and Google in your browser
- Play music from a local folder
- Tell the current time
- Open Visual Studio Code
- Send emails by voice (Gmail)

## Requirements

| Requirement | Details |
|---|---|
| Operating system | Windows 10 or 11 (uses the `sapi5` voice engine and `os.startfile`) |
| Python | 3.10 or newer (3.11 or 3.12 is the safest for PyAudio) |
| Hardware | Working microphone and speakers/headphones |
| Internet | Needed for speech recognition and Wikipedia |
| Editor (optional) | Visual Studio Code with the Python extension |

Python libraries used:

| Library | Purpose |
|---|---|
| `pyttsx3` | Text to speech (Jarvis speaks) |
| `SpeechRecognition` | Converts your voice to text |
| `PyAudio` | Gives Python access to the microphone |
| `wikipedia` | Fetches summaries from Wikipedia |
| `datetime`, `os`, `webbrowser`, `smtplib` | Built into Python, no install needed |

---

## Setup Guide (Step by Step)

### Step 1: Install Python

1. Download Python from <https://www.python.org/downloads/>.
2. Run the installer and **tick "Add Python to PATH"** on the first screen, then click Install.
3. Check the install by opening PowerShell or Command Prompt and running:

   ```bash
   python --version
   pip --version
   ```

   Both should print a version number.

### Step 2: Install Visual Studio Code (recommended)

1. Download VS Code from <https://code.visualstudio.com/>.
2. Open VS Code, go to Extensions (`Ctrl+Shift+X`), search for **Python** (by Microsoft) and install it.

### Step 3: Get the project

Option A: with Git

```bash
git clone https://github.com/your-username/jarvis-ai.git
cd jarvis-ai
```

Option B: without Git. Download the project as a ZIP, extract it, then open the folder in VS Code with **File > Open Folder**.

### Step 4: Create a virtual environment (optional but recommended)

```bash
python -m venv venv
venv\Scripts\activate
```

If PowerShell blocks the activation script, run this once and try again:

```bash
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

### Step 5: Install the libraries

```bash
pip install pyttsx3 SpeechRecognition wikipedia pyaudio
```

Or, if the project has a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

Suggested `requirements.txt`:

```
pyttsx3
SpeechRecognition
wikipedia
pyaudio
comtypes
pywin32
```

**If PyAudio fails to install** (common on the newest Python versions), try one of these:

```bash
pip install pipwin
pipwin install pyaudio
```

or install Python 3.11 or 3.12 and repeat the steps.

### Step 6: Allow microphone access in Windows

1. Open **Settings > Privacy & security > Microphone**.
2. Turn on **Microphone access** and **Let desktop apps access your microphone**.
3. In **Settings > System > Sound**, make sure the correct microphone is selected as the default input device.

### Step 7: Test your setup

Run these two quick checks before starting Jarvis.

Voice test (you should hear the sentence):

```bash
python -c "import pyttsx3; e=pyttsx3.init('sapi5'); e.say('Hello, setup is working'); e.runAndWait()"
```

Microphone test (should print a list of devices, with your mic in it):

```bash
python -c "import speech_recognition as sr; print(sr.Microphone.list_microphone_names())"
```

---

## Configuration

Open `jarvis.py` and change these values for your own computer.

| What | Where in the code | Example |
|---|---|---|
| Music folder | `music_dir` in the `play music` command | `r"D:\Music\Favorites"` |
| VS Code path | `codePath` in the `open code` command | `r"C:\Users\YourName\AppData\Local\Programs\Microsoft VS Code\Code.exe"` |
| Sender email and password | `sendEmail()` function | Your Gmail and an App Password |
| Recipient email | `email to ...` command | The email address you want to send to |
| Wikipedia User-Agent | Near the imports | `wikipedia.wikipedia.USER_AGENT = "JarvisAI/1.0 (your-email)"` |

### Gmail setup for the email feature

1. Turn on **2-Step Verification** in your Google account.
2. Create an **App Password** (Google Account > Security > App passwords).
3. Use that 16-character App Password in `server.login(...)`. Your normal Gmail password will not work.

**Security warning:** never upload your real email address or password to GitHub. Store them in environment variables or a separate config file listed in `.gitignore`.

---

## How to Run the Project

### Method 1: Run from VS Code

1. Open the project folder in VS Code.
2. Open `jarvis.py`.
3. Select the correct Python interpreter (`Ctrl+Shift+P` > **Python: Select Interpreter**).
4. Click the **Run (play) button** at the top right, or press `Ctrl+F5`.

### Method 2: Run from the terminal

```bash
cd "path\to\jarvis ai"
python jarvis.py
```

### What you should see

1. Jarvis speaks a greeting ("good morning", "good afternoon" or "good evening").
2. The terminal prints `Listening...`. Speak your command now.
3. The terminal prints `Recognizing...` and then `User said: <your words>`.
4. Jarvis carries out the command and starts listening again.


## Voice Commands

| Say | What happens |
|---|---|
| "Wikipedia Shahrukh Khan" | Speaks a 2-sentence Wikipedia summary |
| "Open YouTube" | Opens YouTube in your browser |
| "Open Google" | Opens Google in your browser |
| "Play music" | Plays a song from your music folder |
| "The time" | Speaks the current time |
| "Open code" | Launches Visual Studio Code |
| "Email to Harry" | Asks what to say, then sends the email |

---

## Project Structure

```
jarvis ai/
|-- jarvis.py          # Main program
|-- requirements.txt   # Python dependencies (optional)
|-- README.md          # Project documentation
```

## How It Works

| Function | Job |
|---|---|
| `speak(audio)` | Converts text to speech with `pyttsx3` |
| `wishme()` | Greets the user based on the current hour |
| `takeCommand()` | Records from the microphone and returns the spoken words as text |
| `sendEmail(to, content)` | Sends an email through Gmail's SMTP server |
| Main loop (`if __name__ == "__main__"`) | Keeps listening and runs the task matching your words |

Flow:

```
Microphone -> takeCommand() -> text -> if/elif keyword checks -> action -> speak() -> back to listening
```

---

 

## Is It Really AI?

Not in the modern sense. Jarvis uses **speech recognition** and **text-to-speech** services, but the decisions are made with simple `if/elif` keyword checks. It does not understand meaning or learn on its own. It is a great beginner project for learning Python, and you can upgrade it into a real AI assistant by connecting it to a language model.

## Future Improvements

- Add a wake word such as "Hey Jarvis"
- Add an exit command ("stop" or "goodbye")
- Add weather, news and jokes
- Add a graphical interface
- Connect to an AI model for natural conversation

---

 
