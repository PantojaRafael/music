# musicfree
arquivo chamado 
app.py
from flask import Flask, render_template, request
from pytube import YouTube
import shutil
import os

app = Flask(__name__)

def download_music(url):
    try:
        yt = YouTube(url)
        stream = yt.streams.filter(only_audio=True).first()
        stream.download(output_path='./temp', filename=yt.title + ".mp3")

        download_path = os.path.join(os.path.expanduser("~"), "Downloads")
        shutil.move(os.path.join("./temp", yt.title + ".mp3"), os.path.join(download_path, yt.title + ".mp3"))

        return f"Download concluído! Arquivo salvo em Downloads"
    except Exception as e:
        return f"Erro ao baixar: {str(e)}"

@app.route('/')
def index():
    return render_template('index.html', status="")

@app.route('/download', methods=['POST'])
def process_download():
    url = request.form['url']
    status = download_music(url)
    return render_template('index.html', status=status)

if __name__ == '__main__':
    app.run(debug=True)


# Criar uma pasta na raiz do projeto chamada templates e dentro criar um arquivo chamado index.html

arquivo index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MusicFree do YouTube</title>
</head>
<body>
    <h1>MusicFree do YouTube</h1>
    <form action="/download" method="post">
        <label for="url">Cole aqui a URL do vídeo do YouTube:</label>
        <input type="text" id="url" name="url">
        <button type="submit">Baixar</button>
    </form>
    <p>{{ mensagem }}</p>
</body>
</html>

ir até o terminal e escrever .\app.py e dar enter

iniciar o servidor [locallhost: 500 ](http://localhost:5000/)

ir até a URL e digitar localhost:5000

e pronto é sucesso
