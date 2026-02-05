// Mobile menu
const navToggle = document.getElementById("navToggle");
const navMenu = document.getElementById("navMenu");

if (navToggle && navMenu) {
  navToggle.addEventListener("click", () => {
    const open = navMenu.classList.toggle("is-open");
    navToggle.setAttribute("aria-expanded", open ? "true" : "false");
  });

  // close menu on link click (mobile)
  navMenu.querySelectorAll("a").forEach(a => {
    a.addEventListener("click", () => {
      navMenu.classList.remove("is-open");
      navToggle.setAttribute("aria-expanded", "false");
    });
  });
}

// Reveal on scroll
const revealEls = Array.from(document.querySelectorAll(".reveal"));
const io = new IntersectionObserver((entries) => {
  entries.forEach((e) => {
    if (e.isIntersecting) e.target.classList.add("is-visible");
  });
}, { threshold: 0.12 });

revealEls.forEach(el => io.observe(el));

// Lead form: generate message and allow copy
const form = document.getElementById("leadForm");
const outputWrap = document.getElementById("outputWrap");
const outputText = document.getElementById("outputText");
const copyBtn = document.getElementById("copyBtn");

if (form && outputWrap && outputText && copyBtn) {
  form.addEventListener("submit", (ev) => {
    ev.preventDefault();
    const fd = new FormData(form);
    const name = (fd.get("name") || "").toString().trim();
    const city = (fd.get("city") || "").toString().trim();
    const reason = (fd.get("reason") || "").toString().trim();
    const msg = (fd.get("msg") || "").toString().trim();

    const text =
`Olá, Dra. Giselle Ventura! Meu nome é ${name}.
Sou de ${city} e gostaria de falar sobre: ${reason}.
${msg ? `\nMensagem: ${msg}\n` : "\n"}
Como posso solicitar um horário?`;

    outputText.value = text;
    outputWrap.hidden = false;
    copyBtn.disabled = false;
  });

  copyBtn.addEventListener("click", async () => {
    try {
      await navigator.clipboard.writeText(outputText.value);
      copyBtn.textContent = "Copiado ✅";
      setTimeout(() => (copyBtn.textContent = "Copiar"), 1200);
    } catch {
      // fallback: select
      outputText.focus();
      outputText.select();
      document.execCommand("copy");
      copyBtn.textContent = "Copiado ✅";
      setTimeout(() => (copyBtn.textContent = "Copiar"), 1200);
    }
  });
}

// WhatsApp floating button (disabled until we have a number)
const waFloat = document.getElementById("waFloat");
if (waFloat) {
  waFloat.addEventListener("click", () => {});
}
