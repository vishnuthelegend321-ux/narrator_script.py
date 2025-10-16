# narrator_script.py
# --- Step 1: Setup ---
!pip install -q edge-tts
import asyncio
import IPython.display as ipd
import edge_tts

# --- Step 2: Choose your character and paste your text in the form ---

# This dropdown includes the Hindi option.
voice_character = "English (Guy - Narrator)" #@param ["English (Eric)", "English (Guy - Narrator)", "English (Ryan - British)", "English (William - Australian)", "Hindi (Madhur - Narrator)"]

# This is the text box for your input.
text_to_convert = "Paste your text here. Make sure it matches the language of the voice you selected." #@param {type:"string"}


# --- Step 3: Automatically select the correct voice based on your choice ---
VOICE = ""
if voice_character == "English (Eric)":
    VOICE = "en-US-EricNeural"
elif voice_character == "English (Guy - Narrator)":
    VOICE = "en-US-GuyNeural"
elif voice_character == "English (Ryan - British)":
    VOICE = "en-GB-RyanNeural"
elif voice_character == "English (William - Australian)":
    VOICE = "en-AU-WilliamNeural"
elif voice_character == "Hindi (Madhur - Narrator)":
    VOICE = "hi-IN-MadhurNeural"

OUTPUT_FILE = "character_voice_output.mp3"


# --- Step 4: The function that generates the audio ---
async def create_and_play_audio():
    """Converts the text from the form into speech."""
    print(f"🎙️ Generating audio with the voice of {voice_character}...")
    try:
        communicate = edge_tts.Communicate(text_to_convert, VOICE)
        await communicate.save(OUTPUT_FILE)
        print("✅ Success! Your audio file is ready.")
        display(ipd.Audio(OUTPUT_FILE))
    except Exception as e:
        print(f"An error occurred: {e}")

# --- Step 5: Run the process ---
# This ensures the code runs only if a voice has been set and text has been entered.
if VOICE and text_to_convert != "Paste your text here. Make sure it matches the language of the voice you selected.":
    await create_and_play_audio()
else:
    print("👋 Please select a voice, paste your text into the form, and run the cell again.")
