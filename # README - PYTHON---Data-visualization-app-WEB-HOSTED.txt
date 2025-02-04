CHATGPT CONVERSATION

***********


You said:
create a web hosted DATA VISUALIZATION app in Python
ChatGPT said:
ChatGPT
Here's a basic web-hosted data visualization app using Python with Flask and Plotly. It allows users to upload CSV files and visualize data through interactive charts.

Data Viz App

Answer in chat instead
This Flask app allows users to upload CSV files and view a scatter plot of the first two columns. It uses Plotly for visualization. Let me know if you want additional features like different chart types or user customization! 🚀

***********

from flask import Flask, render_template, request
import pandas as pd
import plotly.express as px
import os

app = Flask(__name__)
UPLOAD_FOLDER = 'uploads'
os.makedirs(UPLOAD_FOLDER, exist_ok=True)
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        file = request.files['file']
        if file:
            filepath = os.path.join(app.config['UPLOAD_FOLDER'], file.filename)
            file.save(filepath)
            df = pd.read_csv(filepath)
            return render_template('index.html', tables=[df.head().to_html()], filename=file.filename)
    return render_template('index.html', tables=None, filename=None)

@app.route('/visualize/<filename>', methods=['GET'])
def visualize(filename):
    filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
    df = pd.read_csv(filepath)
    fig = px.scatter(df, x=df.columns[0], y=df.columns[1], title='Scatter Plot')
    graph_html = fig.to_html(full_html=False)
    return render_template('visualization.html', graph_html=graph_html)

if __name__ == '__main__':
    app.run(debug=True)
