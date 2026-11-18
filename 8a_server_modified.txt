''' flask server to run emotion detection app '''
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector as detect_emotion

app = Flask("Emotion Detection")

@app.route("/")
def index():
    ''' Load the index page '''
    return render_template("index.html")

@app.route("/emotionDetector")
def emotion_detector():
    ''' Function to take user input and analyze the input '''

    text_to_analyze = request.args.get('textToAnalyze')

    response = detect_emotion(text_to_analyze)

    if response['dominant_emotion'] is None:
        return "Invalid text! Please try again!"

    return response

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
