# Steps to capture in the IBM Skills Network Cloud IDE

The Watson NLP endpoint (`sn-watson-emotion.labs.skills.network`) only answers requests
made from inside the course's Cloud IDE. Everything else in this project is finished,
pushed, and verified locally with mocked responses. Do the following once, inside the
Cloud IDE, to capture the three network-dependent terminal outputs and two screenshots.

## 0. Get the code into the Cloud IDE
```bash
mkdir final_project
git clone https://github.com/Lbstrydom/oaqjp-final-project-emb-ai.git final_project
cd final_project
pip install -r requirements.txt
```

## 2b_application_creation (terminal output)
Open a python3 shell and paste this (reproduces the original unpackaged Task-2 function
inline, so nothing in the repo needs editing):
```python
import requests

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    headers = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    input_json = { "raw_document": { "text": text_to_analyze } }
    response = requests.post(url, json=input_json, headers=headers)
    return response.text

emotion_detector("I love this new technology.")
```
Copy the shell input+output into a text file named `2b_application_creation`.

## 3b_formatted_output_test (terminal output)
In the same shell (or a new one), paste this (the Task-3 formatted version, still
inline — no repo edits needed):
```python
import json, requests

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    headers = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    input_json = { "raw_document": { "text": text_to_analyze } }
    response = requests.post(url, json=input_json, headers=headers)
    formatted_response = json.loads(response.text)
    emotions = formatted_response['emotionPredictions'][0]['emotion']
    scores = {k: emotions[k] for k in ('anger', 'disgust', 'fear', 'joy', 'sadness')}
    scores['dominant_emotion'] = max(
        (e for e in scores if e != 'dominant_emotion'), key=scores.get
    )
    return scores

emotion_detector("I am so happy I am doing this.")
```
Verify `dominant_emotion` is `joy`. Copy shell input+output into `3b_formatted_output_test`.

## 4b_packaging_test (terminal output)
This uses the real package already in the repo — no inline copy needed:
```python
from EmotionDetection.emotion_detection import emotion_detector
emotion_detector("I hate working long hours.")
```
Verify `dominant_emotion` is `anger`. Copy shell input+output into `4b_packaging_test`.

## 5b_unit_testing_result (terminal output)
```bash
python3 test_emotion_detection.py -v
```
Confirm all 5 assertions pass (each dominant emotion matches). Save the output as
`5b_unit_testing_result`.

## 6b_deployment_test.png (screenshot)
```bash
python3 server.py
```
Use the Cloud IDE's "Launch Application" (port 5000), open the page, type
`I think I am having fun` into the box, click **Run Sentiment Analysis**, and
screenshot the rendered response. Save as `6b_deployment_test.png`.
(A locally-generated mock preview of this exact screen already lives at
`submission/6b_deployment_test_LOCAL_MOCK_DEMO.png` — same UI, fabricated
scores — for your reference only; it is not a valid submission artifact.)

## 7c_error_handling_interface.png (screenshot)
With `server.py` still running, clear the text box (leave it blank) and click
**Run Sentiment Analysis** again. Screenshot the "Invalid text! Please try again!"
response. Save as `7c_error_handling_interface.png`.
(Local mock preview: `submission/7c_error_handling_interface_LOCAL_MOCK_DEMO.png`.)
