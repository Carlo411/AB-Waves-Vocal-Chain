document.addEventListener("DOMContentLoaded", () => {
  const players = document.querySelectorAll(".player__wrapper");

  players.forEach((player) => {
    const audioA = new Audio(player.dataset.audioA);
    const audioB = new Audio(player.dataset.audioB);

    const playBtn = player.querySelector(".play__button");
    const stopBtn = player.querySelector(".stop__button");
    const btnA = player.querySelector(".a__button");
    const btnB = player.querySelector(".b__button");
    const progressFill = player.querySelector(".progress__fill");
    const progressContainer = player.querySelector(".progress__container");

    let currentAudio = audioA;
    let isPlaying = false;

    let loadedA = false;
    let loadedB = false;

    audioA.addEventListener("canplaythrough", () => {
      loadedA = true;
      checkReady();
    });

    audioB.addEventListener("canplaythrough", () => {
      loadedB = true;
      checkReady();
    });

    function checkReady() {
      if (loadedA && loadedB) {
        btnA.disabled = false;
        btnB.disabled = false;
        playBtn.disabled = false;
        stopBtn.disabled = false;
      }
    }

    btnA.addEventListener("click", () => {
      if (isPlaying) currentAudio.pause();
      currentAudio = audioA;
      if (isPlaying) currentAudio.play();
      updateButtons();
    });

    btnB.addEventListener("click", () => {
      if (isPlaying) currentAudio.pause();
      currentAudio = audioB;
      if (isPlaying) currentAudio.play();
      updateButtons();
    });

    playBtn.addEventListener("click", () => {
      if (!isPlaying) {
        currentAudio.play();
        isPlaying = true;
        playBtn.innerHTML = '<i class="fa-solid fa-pause"></i>';
      } else {
        currentAudio.pause();
        isPlaying = false;
        playBtn.innerHTML = '<i class="fa-solid fa-play"></i>';
      }
    });

    stopBtn.addEventListener("click", () => {
      currentAudio.pause();
      currentAudio.currentTime = 0;
      isPlaying = false;
      playBtn.innerHTML = '<i class="fa-solid fa-play"></i>';
      updateProgress(0);
    });

    currentAudio.addEventListener("timeupdate", () => {
      const progressPercent = (currentAudio.currentTime / currentAudio.duration) * 100;
      updateProgress(progressPercent);
    });

    progressContainer.addEventListener("click", (e) => {
      const width = progressContainer.clientWidth;
      const clickX = e.offsetX;
      const duration = currentAudio.duration;

      currentAudio.currentTime = (clickX / width) * duration;
    });

    function updateProgress(percent) {
      progressFill.style.width = `${percent}%`;
    }

    function updateButtons() {
      btnA.classList.toggle("active", currentAudio === audioA);
      btnB.classList.toggle("active", currentAudio === audioB);
    }

    updateButtons();
  });
});
