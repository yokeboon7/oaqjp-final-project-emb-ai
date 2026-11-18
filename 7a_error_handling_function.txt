import requests
import json

def emotion_detector(text_to_analyze):
    ''' Emotion detector is a function to analyze user input '''

    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    params = { "raw_document": { "text": text_to_analyze } }

    response = requests.post(url, json=params, headers=header)

    if response.status_code == 200:
        formatted_response = json.loads(response.text)
        emotions = formatted_response['emotionPredictions'][0]['emotion']

        # Get the dominant emotion based on its score
        dominant_emotion = max(emotions, key=emotions.get)

        joy = emotions['joy']
        anger = emotions['anger']
        disgust = emotions['disgust']
        fear = emotions['fear']
        sadness = emotions['sadness']
    else:
        dominant_emotion = None
        joy = None
        anger = None
        disgust = None
        fear = None
        sadness = None


    return {
        'anger': anger,
        'disgust': disgust,
        'fear': fear,
        'joy': joy,
        'sadness': sadness,
        'dominant_emotion': dominant_emotion
    }