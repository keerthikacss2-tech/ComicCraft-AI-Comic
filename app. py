import streamlit as st
import google.generativeai as genai
from PIL import Image, ImageDraw
import textwrap

st.set_page_config(page_title="ComicCraft - Gemini Comic Creator", layout="wide")
st.title("⚡ ComicCraft - AI Comic Story Creator")
st.write("Gemini model use panni un idea la irunthu full comic story ah create pannuvom!")

# API Key
api_key = st.sidebar.text_input("Gemini API Key:", type="password")
if api_key:
    genai.configure(api_key=api_key)

prompt = st.text_area("Un Comic Idea:", "A robot dog who wants to become a hero in Chennai", height=120)
style = st.selectbox("Comic Style:", ["Manga", "Superhero", "Funny", "Dark"])
num_panels = st.slider("Enna panels venum?", 3, 6, 4)

if st.button("✨ Create Comic with Gemini"):
    if not api_key:
        st.error("Mela sidebar la Gemini API Key kodu!")
    elif not prompt:
        st.warning("Idea type pannu!")
    else:
        with st.spinner("Gemini story ezhuthu... wait pannu"):
            model = genai.GenerativeModel("gemini-1.5-flash")
            full_prompt = f"You are a comic writer. Create a {style} comic story for idea: '{prompt}' in {num_panels} panels. For each panel give only one short dialogue/caption, max 15 words. Format: Panel 1:... | Panel 2:... etc."
            response = model.generate_content(full_prompt)
            story_text = response.text

        st.subheader("Gemini Story:")
        st.write(story_text)

        # Panels ah split pannu
        parts = story_text.replace("Panel", "|Panel").split("|")
        panels = [p.strip() for p in parts if "Panel" in p][:num_panels]

        st.subheader("🎨 Un Comic:")
        cols = st.columns(len(panels))
        for i, col in enumerate(cols):
            with col:
                text = panels[i] if i < len(panels) else f"Panel {i+1}"
                img = Image.new('RGB', (500, 500), color=(255, 255, 240))
                draw = ImageDraw.Draw(img)
                draw.rectangle([5, 5, 495, 495], outline="black", width=6)
                draw.rectangle([5, 5, 495, 80], fill="black")
                draw.text((20, 20), f"PANEL {i+1}", fill="yellow")
                wrapped = textwrap.wrap(text, width=22)
                y = 150
                for line in wrapped:
                    draw.text((50, y), line, fill="black")
                    y += 35
                st.image(img, use_column_width=True)
