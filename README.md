!pip install -q -U google-generativeai
import os
import google.generativeai as genai

# Securely load your API key
GOOGLE_API_KEY='AIzaSyAxUoSzIRQz_jqjEo0ajeCxQ35uto_xEpc'
genai.configure(api_key=GOOGLE_API_KEY)

# Model 1 Configuration 
generation_config1 = {
    'candidate_count': 1,
    'temperature': 0.8, 
    'top_p': 0.6,
    'top_k': 90,
}
safety_settings1 = {
    'harassment': 'block_none',
    'hate': 'block_none',
    'sexual': 'block_none',
    'dangerous': 'block_none',
}
model1 = genai.GenerativeModel(
    model_name='gemini-1.0-pro',
    generation_config=generation_config1,
    safety_settings=safety_settings1
)
chat1 = model1.start_chat(history=[]) 

# Model 2 Configuration
generation_config2 = {
    'candidate_count': 1,
    'temperature': 0.9,
    'top_p': 0.5,
    'top_k': 100,
}
safety_settings2 = {
    'harassment': 'block_none',
    'hate': 'block_none',
    'sexual': 'block_none',
    'dangerous': 'block_none',
}
model2 = genai.GenerativeModel(
    model_name='gemini-1.0-pro',
    generation_config=generation_config2,
    safety_settings=safety_settings2
)
chat2 = model2.start_chat(history=[]) 

# Conversation Loop
last_response = "Hello, how are you?" 
previous_responses = []

while True:
    response = chat1.send_message(last_response)
    print('AI 1: ', response1.text, '\n')
    previous_responses.append(response1.text)

    # Basic repetition prevention 
    prompt_for_model2 = response1.text
    for prev_response in previous_responses:
        if prompt_for_model2 == prev_response:
            prompt_for_model2 += " Tell me something different."
            break

    response2 = chat2.send_message(prompt_for_model2)
    print('AI 2: ', response2.text, '\n')
    previous_responses.append(response2.text)
    last_response = response2.text

    if "goodbye" in last_response.lower():
        break
        
