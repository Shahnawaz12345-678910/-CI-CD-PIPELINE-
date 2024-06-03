THATS  a Basic HTML + CSS + JAVASCRIPT website code of music-player thats deploy by me.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Animated Music Player</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: linear-gradient(135deg, #ff4e50, #f9d423);
            margin: 0;
        }

        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            padding: 20px;
            max-width: 400px;
            width: 100%;
            text-align: center;
            animation: fadeIn 1s ease-out;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: scale(0.8);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }

        .player {
            display: flex;
            align-items: center;
            justify-content: space-between;
            width: 100%;
            margin-bottom: 20px;
        }

        .player button {
            background: #ff4e50;
            border: none;
            border-radius: 50%;
            color: white;
            width: 50px;
            height: 50px;
            font-size: 20px;
            cursor: pointer;
            transition: background 0.3s;
        }

        .player button:hover {
            background: #e84347;
        }

        .progress {
            flex-grow: 1;
            height: 10px;
            background-color: #ddd;
            border-radius: 5px;
            overflow: hidden;
            margin: 0 20px;
            position: relative;
        }

        .progress-bar {
            height: 100%;
            background-color: #4CAF50;
            width: 0;
            transition: width 0.25s;
        }

        .songs-list {
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 100%;
        }

        .song {
            width: 100%;
            margin: 10px 0;
            background: #fff;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            transition: transform 0.3s;
        }

        .song:hover {
            transform: scale(1.05);
        }

        .song img {
            width: 100%;
            border-bottom: 1px solid #ddd;
        }

        .play-song {
            width: 100%;
            padding: 15px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            font-size: 16px;
            transition: background 0.3s;
        }

        .play-song:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="player">
            <button class="prev">&#9664;</button>
            <button class="play">&#9654;</button>
            <button class="next">&#9654;&#9654;</button>
            <div class="progress">
                <div class="progress-bar"></div>
            </div>
        </div>
        <div class="songs-list">
            <div class="song">
                <img src="img/baarish.jpg" alt="Baarish">
                <button class="play-song" data-song="songs/baarish.mp3">Play</button>
            </div>
            <div class="song">
                <img src="img/tuhi.jpg" alt="Tu Hi Hai">
                <button class="play-song" data-song="songs/tuhi.mp3">Play</button>
            </div>
            <div class="song">
                <img src="img/phirbhi.jpg" alt="Phir Bhi Tumko Chaahunga">
                <button class="play-song" data-song="songs/phirbhi.mp3">Play</button>
            </div>
            <div class="song">
                <img src="img/thodaaur.jpg" alt="Thodi Der">
                <button class="play-song" data-song="songs/thodaaur.mp3">Play</button>
            </div>
            <div class="song">
                <img src="img/stayawara.jpg" alt="Stay A Little Longer">
                <button class="play-song" data-song="songs/stayawara.mp3">Play</button>
            </div>
        </div>
    </div>
    <script>
        const songs = [
            {
                name: 'songs/baarish.mp3',
                img: 'img/baarish.jpg'
            },
            {
                name: 'songs/tuhi.mp3',
                img: 'img/tuhi.jpg'
            },
            {
                name: 'songs/phirbhi.mp3',
                img: 'img/phirbhi.jpg'
            },
            {
                name: 'songs/thodaaur.mp3',
                img: 'img/thodaaur.jpg'
            },
            {
                name: 'songs/stayawara.mp3',
                img: 'img/stayawara.jpg'
            }
        ];

        const audioElement = document.createElement('audio');
        document.body.appendChild(audioElement);

        const playButton = document.querySelector('.play');
        const prevButton = document.querySelector('.prev');
        const nextButton = document.querySelector('.next');
        const progressBar = document.querySelector('.progress-bar');

        let songIndex = 0;

        function loadSong(song) {
            audioElement.src = song.name;
            progressBar.style.width = "0%";
        }

        function playSong() {
            audioElement.play();
            playButton.innerHTML = '&#10074;&#10074;';
        }

        function pauseSong() {
            audioElement.pause();
            playButton.innerHTML = '&#9654;';
        }

        function prevSong() {
            songIndex--;
            if (songIndex < 0) {
                songIndex = songs.length - 1;
            }
            loadSong(songs[songIndex]);
            playSong();
        }

        function nextSong() {
            songIndex++;
            if (songIndex >= songs.length) {
                songIndex = 0;
            }
            loadSong(songs[songIndex]);
            playSong();
        }

        playButton.addEventListener('click', () => {
            if (audioElement.paused) {
                playSong();
            } else {
                pauseSong();
            }
        });

        prevButton.addEventListener('click', prevSong);
        nextButton.addEventListener('click', nextSong);

        document.querySelectorAll('.play-song').forEach(button => {
            button.addEventListener('click', () => {
                const songName = button.getAttribute('data-song');
                const song = songs.find(s => s.name === songName);
                loadSong(song);
                playSong();
            });
        });

        audioElement.addEventListener('timeupdate', () => {
            const progressPercent = (audioElement.currentTime / audioElement.duration) * 100;
            progressBar.style.width = `${progressPercent}%`;
        });

        // Load the first song initially
        loadSong(songs[songIndex]);
    </script>
</body>
</html>
