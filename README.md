# test
from flask import Flask, render_template_string, request
import openai

# Ganti pakai API KEY kamu
openai.api_key = "sk-ISI_API_KEY_KAMU"

app = Flask(__name__)

html_template = """
<!doctype html>
<html>
<head>
    <title>AI ChatBot Web</title>
</head>
<body style="background:#000;color:#0ff;font-family:sans-serif;padding:20px;">
    <h1>AI ChatBot</h1>
    <form method="post">
        <input name="message" style="width:70%;" placeholder="Ketik pesan...">
        <input type="submit" value="Kirim">
    </form>
    {% if response %}
    <p><b>Bot:</b> {{ response }}</p>
    {% endif %}
</body>
</html>
"""

@app.route("/", methods=["GET", "POST"])
def chat():
    response = ""
    if request.method == "POST":
        user_message = request.form["message"]

        reply = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[
                {"role": "user", "content": user_message}
            ]
        )
        response = reply['choices'][0]['message']['content']
    return render_template_string(html_template, response=response)

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
