<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vitvisor Video Hub</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background-color: #0a0a0a;
        color: #00ffff;
        margin: 0;
        padding: 0;
    }
    header {
        background-color: #001f2d;
        padding: 20px;
        text-align: center;
        font-size: 2rem;
        font-weight: bold;
        color: #00ffff;
    }
    .search-container {
        text-align: center;
        margin: 20px 0;
    }
    .search-container input[type="text"] {
        padding: 10px;
        width: 60%;
        max-width: 400px;
        border-radius: 5px;
        border: none;
        outline: none;
        font-size: 1rem;
    }
    .videos-container {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
        gap: 20px;
        padding: 20px;
    }
    .video-card {
        background-color: #001f2d;
        padding: 10px;
        border-radius: 10px;
        cursor: pointer;
        transition: transform 0.3s;
    }
    .video-card:hover {
        transform: scale(1.05);
    }
    video {
        width: 100%;
        height: 180px;
        border-radius: 5px;
        background-color: black;
    }
    /* Modal */
    .modal {
        display: none;
        position: fixed;
        top:0;
        left:0;
        width: 100%;
        height: 100%;
        background-color: rgba(0,0,0,0.9);
        justify-content: center;
        align-items: center;
        z-index: 1000;
    }
    .modal-content {
        width: 80%;
        max-width: 900px;
        height: 60%;
    }
    .modal video {
        width: 100%;
        height: 100%;
    }
    .close-btn {
        position: absolute;
        top: 20px;
        right: 40px;
        font-size: 2rem;
        color: #00ffff;
        cursor: pointer;
    }
</style>
</head>
<body>

<header>Vitvisor</header>

<div class="search-container">
    <input type="text" id="searchInput" placeholder="Buscar vídeos...">
</div>

<div class="videos-container" id="videosContainer">
    <div class="video-card" data-src="lerica.mp4" data-title="Video 1"><video src="lerica.mp4" controls></video><p>lerica</p></div>
    <div class="video-card" data-src="videos/video2.mp4" data-title="Video 2"><video src="videos/video2.mp4" controls></video><p>Video 2</p></div>
    <div class="video-card" data-src="videos/video3.mp4" data-title="Video 3"><video src="videos/video3.mp4" controls></video><p>Video 3</p></div>
    <div class="video-card" data-src="videos/video4.mp4" data-title="Video 4"><video src="videos/video4.mp4" controls></video><p>Video 4</p></div>
    <div class="video-card" data-src="videos/video5.mp4" data-title="Video 5"><video src="videos/video5.mp4" controls></video><p>Video 5</p></div>
    <div class="video-card" data-src="videos/video6.mp4" data-title="Video 6"><video src="videos/video6.mp4" controls></video><p>Video 6</p></div>
    <div class="video-card" data-src="videos/video7.mp4" data-title="Video 7"><video src="videos/video7.mp4" controls></video><p>Video 7</p></div>
    <div class="video-card" data-src="videos/video8.mp4" data-title="Video 8"><video src="videos/video8.mp4" controls></video><p>Video 8</p></div>
    <div class="video-card" data-src="videos/video9.mp4" data-title="Video 9"><video src="videos/video9.mp4" controls></video><p>Video 9</p></div>
    <div class="video-card" data-src="videos/video10.mp4" data-title="Video 10"><video src="videos/video10.mp4" controls></video><p>Video 10</p></div>
    <div class="video-card" data-src="videos/video11.mp4" data-title="Video 11"><video src="videos/video11.mp4" controls></video><p>Video 11</p></div>
    <div class="video-card" data-src="videos/video12.mp4" data-title="Video 12"><video src="videos/video12.mp4" controls></video><p>Video 12</p></div>
    <div class="video-card" data-src="videos/video13.mp4" data-title="Video 13"><video src="videos/video13.mp4" controls></video><p>Video 13</p></div>
    <div class="video-card" data-src="videos/video14.mp4" data-title="Video 14"><video src="videos/video14.mp4" controls></video><p>Video 14</p></div>
    <div class="video-card" data-src="videos/video15.mp4" data-title="Video 15"><video src="videos/video15.mp4" controls></video><p>Video 15</p></div>
    <div class="video-card" data-src="videos/video16.mp4" data-title="Video 16"><video src="videos/video16.mp4" controls></video><p>Video 16</p></div>
    <div class="video-card" data-src="videos/video17.mp4" data-title="Video 17"><video src="videos/video17.mp4" controls></video><p>Video 17</p></div>
    <div class="video-card" data-src="videos/video18.mp4" data-title="Video 18"><video src="videos/video18.mp4" controls></video><p>Video 18</p></div>
    <div class="video-card" data-src="videos/video19.mp4" data-title="Video 19"><video src="videos/video19.mp4" controls></video><p>Video 19</p></div>
    <div class="video-card" data-src="videos/video20.mp4" data-title="Video 20"><video src="videos/video20.mp4" controls></video><p>Video 20</p></div>
</div>

<div class="modal" id="videoModal">
    <span class="close-btn" id="closeModal">&times;</span>
    <div class="modal-content">
        <video id="modalVideo" controls autoplay></video>
    </div>
</div>

<script>
    const searchInput = document.getElementById('searchInput');
    const videoCards = Array.from(document.getElementsByClassName('video-card'));
    const modal = document.getElementById('videoModal');
    const modalVideo = document.getElementById('modalVideo');
    const closeModal = document.getElementById('closeModal');

    let currentIndex = -1;

    searchInput.addEventListener('input', () => {
        const query = searchInput.value.toLowerCase();
        videoCards.forEach(card => {
            const title = card.dataset.title.toLowerCase();
            card.style.display = title.includes(query) ? 'block' : 'none';
        });
    });

    videoCards.forEach((card, index) => {
        card.addEventListener('click', () => {
            currentIndex = index;
            openVideo(card.dataset.src);
        });
    });

    function openVideo(src) {
        modal.style.display = 'flex';
        modalVideo.src = src;
        modalVideo.play();
    }

    closeModal.addEventListener('click', () => {
        modal.style.display = 'none';
        modalVideo.pause();
        modalVideo.src = '';
    });

    modal.addEventListener('click', e => {
        if(e.target === modal) {
            modal.style.display = 'none';
            modalVideo.pause();
            modalVideo.src = '';
        }
    });

    // 🔥 Navegación con flechas
    document.addEventListener('keydown', e => {
        if (modal.style.display !== 'flex') return;

        if (e.key === 'ArrowRight') {
            currentIndex = (currentIndex + 1) % videoCards.length;
            openVideo(videoCards[currentIndex].dataset.src);
        }

        if (e.key === 'ArrowLeft') {
            currentIndex = (currentIndex - 1 + videoCards.length) % videoCards.length;
            openVideo(videoCards[currentIndex].dataset.src);
        }
    });
</script>

</body>
</html>
