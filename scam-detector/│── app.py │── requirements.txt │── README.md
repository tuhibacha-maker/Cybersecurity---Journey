import streamlit as st
import re

st.set_page_config(page_title="AI Scam Detector", page_icon="⚠️")

st.title("⚠️ AI Scam Message Detector")
st.write("Paste any message and check if it's scam or safe")

# scam keywords list
scam_keywords = [
    "urgent", "lottery", "win money", "free", "click now",
    "bank account", "password", "otp", "verify", "congratulations",
    "claim reward", "limited time", "loan approved", "cash prize"
]

def check_scam(text):
    text = text.lower()
    score = 0

    for word in scam_keywords:
        if word in text:
            score += 1

    # extra rule: links
    if re.search(r"http|www|\.com", text):
        score += 1

    if score == 0:
        return "✅ SAFE", score
    elif score <= 2:
        return "⚠️ SUSPICIOUS", score
    else:
        return "🚨 SCAM", score

user_input = st.text_area("Enter message here:")

if st.button("Check"):
    if user_input.strip() == "":
        st.warning("Please enter a message")
    else:
        result, score = check_scam(user_input)
        st.subheader(result)
        st.write("Risk Score:", score)
