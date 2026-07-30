/* ===================================================
   DMS STAR — общие скрипты (nav, fade-in, copy IP)
   =================================================== */

document.addEventListener('DOMContentLoaded', () => {
  // Мобильное меню
  const burger = document.getElementById('burger');
  const mobileMenu = document.getElementById('mobileMenu');
  if (burger && mobileMenu) {
    burger.addEventListener('click', () => {
      burger.classList.toggle('open');
      mobileMenu.classList.toggle('open');
    });
    mobileMenu.querySelectorAll('a').forEach(link => {
      link.addEventListener('click', () => {
        burger.classList.remove('open');
        mobileMenu.classList.remove('open');
      });
    });
  }

  // Fade-in при скролле
  const fadeObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
        fadeObserver.unobserve(entry.target);
      }
    });
  }, { threshold: 0.1 });
  document.querySelectorAll('.fade-in').forEach(el => fadeObserver.observe(el));

  // Копирование IP
  document.querySelectorAll('.copy-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      const ip = btn.dataset.ip || (btn.previousElementSibling ? btn.previousElementSibling.textContent.trim() : '');
      if (!ip) return;
      navigator.clipboard.writeText(ip).then(() => {
        const original = btn.textContent;
        btn.textContent = 'Скопировано';
        btn.classList.add('copied');
        setTimeout(() => {
          btn.textContent = original;
          btn.classList.remove('copied');
        }, 1800);
      }).catch(() => {
        // Фолбэк для браузеров без Clipboard API
        const ta = document.createElement('textarea');
        ta.value = ip;
        document.body.appendChild(ta);
        ta.select();
        try { document.execCommand('copy'); } catch (e) {}
        document.body.removeChild(ta);
        const original = btn.textContent;
        btn.textContent = 'Скопировано';
        btn.classList.add('copied');
        setTimeout(() => {
          btn.textContent = original;
          btn.classList.remove('copied');
        }, 1800);
      });
    });
  });

  // FAQ аккордеон
  document.querySelectorAll('.accordion-q').forEach(q => {
    q.addEventListener('click', () => {
      const item = q.closest('.accordion-item');
      item.classList.toggle('open');
    });
  });

  // Фильтр медиа (страница "Контент")
  const filterBtns = document.querySelectorAll('.media-filter button');
  const mediaCards = document.querySelectorAll('.media-card');
  if (filterBtns.length && mediaCards.length) {
    filterBtns.forEach(btn => {
      btn.addEventListener('click', () => {
        filterBtns.forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        const filter = btn.dataset.filter;
        mediaCards.forEach(card => {
          card.style.display = (filter === 'all' || card.dataset.type === filter) ? '' : 'none';
        });
      });
    });
  }
});
