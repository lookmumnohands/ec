function initFloatingPreview() {
  const isMobile = window.matchMedia("(pointer: coarse)").matches;
  if (isMobile) return;

  const rows = document.querySelectorAll('.text-row');
  const preview = document.getElementById('floating-preview');
  if (!preview || rows.length === 0) return;

  let currentMedia = null;
  let textTimeout = null;
  let mediaTimeout = null;

  rows.forEach(row => {
    const imgUrl = row.dataset.img;
    const videoUrl = row.dataset.video;

    row.addEventListener('mouseenter', () => {
      clearTimeout(textTimeout);
      clearTimeout(mediaTimeout);

      row.dataset.originalColor = row.style.color;

      textTimeout = setTimeout(() => {
        row.style.color = 'transparent';
      }, 50);

      mediaTimeout = setTimeout(() => {
        if (currentMedia) {
          currentMedia.remove();
          currentMedia = null;
        }

        const rect = row.getBoundingClientRect();
        const scrollY = window.scrollY;
        const topPosition = rect.top + scrollY + rect.height / 2;

        if (imgUrl) {
          currentMedia = document.createElement('img');
          currentMedia.src = imgUrl;
        } else if (videoUrl) {
          currentMedia = document.createElement('video');
          currentMedia.src = videoUrl;
          currentMedia.autoplay = true;
          currentMedia.loop = true;
          currentMedia.muted = true;
          currentMedia.playsInline = true;
        }

        if (currentMedia) {
          currentMedia.style.opacity = '1';
          preview.style.top = `${topPosition}px`;
          preview.innerHTML = '';
          preview.appendChild(currentMedia);
        }
      }, 300);
    });

    row.addEventListener('mouseleave', () => {
      clearTimeout(textTimeout);
      clearTimeout(mediaTimeout);
      row.style.color = row.dataset.originalColor || '#000';

      if (currentMedia) {
        currentMedia.style.opacity = '0';
        const temp = currentMedia;
        setTimeout(() => {
          if (temp && preview.contains(temp)) {
            temp.remove();
            if (currentMedia === temp) currentMedia = null;
          }
        }, 400);
      }
    });
  });
}

// 🔁 Reintenta fins que la pàgina estigui llesta
function waitForTextRowsAndPreview(attempts = 0) {
  const maxAttempts = 20;
  const preview = document.getElementById('floating-preview');
  const rows = document.querySelectorAll('.text-row');

  if (preview && rows.length > 0) {
    initFloatingPreview();
  } else if (attempts < maxAttempts) {
    setTimeout(() => waitForTextRowsAndPreview(attempts + 1), 300);
  }
}

document.addEventListener("DOMContentLoaded", waitForTextRowsAndPreview);
