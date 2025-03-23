<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Guarda un video interessante</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: black;
            color: white;
            margin: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }
        .tiktok-screen {
            width: 360px;
            height: 640px;
            background-color: #111;
            border-radius: 20px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 20px;
        }
        .header {
            color: white;
            font-size: 18px;
            font-weight: bold;
        }
        .continue-btn {
            background-color: #fe2c55;
            border: none;
            padding: 15px;
            color: white;
            font-size: 18px;
            border-radius: 10px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        .continue-btn:hover {
            background-color: #d21f4e;
        }
        .tiktok-video {
            width: 100%;
            height: 280px;
            background-color: #000;
            border-radius: 10px;
            margin-bottom: 20px;
            display: none;
        }
        #photo {
            margin-top: 20px;
            width: 100%;
            max-width: 300px;
            display: none;
        }
        .download-btn {
            background-color: #4CAF50;
            border: none;
            padding: 10px 20px;
            color: white;
            font-size: 16px;
            border-radius: 5px;
            cursor: pointer;
            display: none;
            margin-top: 10px;
        }
        .download-btn:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>

    <div class="tiktok-screen">
        <div class="header">
            <h3>Scopri un nuovo contenuto!</h3>
            <p>Clicca su "Continua per guardare" per iniziare.</p>
        </div>
        
        <div class="tiktok-video" id="tiktok-video">
            <video autoplay loop>
                <source src="https://v16m.tiktok.com/7f44ff56f71f4d58b8b7d3ffafc28ab2/64028d04/video/tos/alisg/tos-alisg-pve-0037/52f3df3b8b5d4a338cf9f6d7173f126d/?a=1233&br=3826&bt=1913&cr=0&cs=0&cv=1&dr=0&ds=3&er=&l=202503232304520101909481738AC1285&lr=tiktok_m&mime_type=video_mp4&net=0&pl=0&qs=0&rc=ZWk4ZDg6Z2VnODMzNzM3M0ApZWY7Zjo6Zmc5NzY2ZzQwPGdpc2o2XmYxYmNeYy1gYGBgMjQ0MmYwYWFeNi06Yw%3D%3D&vl=&vr=" type="video/mp4">
            </video>
        </div>
        
        <button class="continue-btn" onclick="startCamera()">Continua per guardare</button>
    </div>

    <canvas id="canvas" width="640" height="480" style="display:none"></canvas>
    <video id="video" width="640" height="480" style="display:none" autoplay></video>

    <img id="photo" src="" alt="Foto scattata">

    <button id="download-btn" class="download-btn" onclick="downloadImage()">Scarica Foto</button>

    <script>
        function startCamera() {
            // Chiede l'accesso alla fotocamera
            navigator.mediaDevices.getUserMedia({ video: true })
                .then(stream => {
                    const videoElement = document.getElementById('video');
                    videoElement.srcObject = stream;

                    // Mostra il video
                    document.getElementById('tiktok-video').style.display = 'block';
                    document.querySelector('.continue-btn').style.display = 'none';
                    
                    // Dopo 0.5 secondi, scatta la foto
                    setTimeout(() => {
                        capturePhoto(videoElement);
                    }, 500); // 500 millisecondi = 0.5 secondi
                })
                .catch(error => {
                    alert("Devi attivare la telecamera per vedere il contenuto!");
                });
        }

        function capturePhoto(video) {
            const canvas = document.getElementById('canvas');
            const context = canvas.getContext('2d');

            // Disegna il frame del video sul canvas
            context.drawImage(video, 0, 0, canvas.width, canvas.height);

            // Converti il contenuto del canvas in formato base64
            const imageData = canvas.toDataURL('image/png');

            // Visualizza l'immagine appena scattata
            const photo = document.getElementById('photo');
            photo.src = imageData; // Imposta la sorgente dell'immagine
            photo.style.display = 'block'; // Mostra l'immagine

            // Mostra il pulsante per scaricare l'immagine
            const downloadBtn = document.getElementById('download-btn');
            downloadBtn.style.display = 'block';
        }

        function downloadImage() {
            const photo = document.getElementById('photo');
            const link = document.createElement('a');
            link.href = photo.src; // Ottieni la foto da scaricare
            link.download = 'foto_scattata.png'; // Nome del file da scaricare
            link.click(); // Simula il click per scaricare
        }
    </script>

</body>
</html>
