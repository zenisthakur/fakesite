(https://github.com/user-attachments/files/25457540/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Minimal Shop Gallery</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: system-ui, -apple-system, sans-serif;
      background: #f8f9fa;
      color: #1a1a1a;
      line-height: 1.6;
    }

    header {
      padding: 2.5rem 1rem 1.5rem;
      text-align: center;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
    }

    h1 {
      font-size: 2.4rem;
      margin-bottom: 0.4rem;
    }

    .subtitle {
      opacity: 0.9;
      font-size: 1.1rem;
    }

    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 2rem 1rem;
    }

    .gallery {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
      gap: 1.8rem;
      padding: 1rem 0;
    }

    .card {
      background: white;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 4px 15px rgba(0,0,0,0.08);
      transition: all 0.25s ease;
      cursor: pointer;
    }

    .card:hover {
      transform: translateY(-8px);
      box-shadow: 0 12px 30px rgba(0,0,0,0.12);
    }

    .card img {
      width: 100%;
      height: 280px;
      object-fit: cover;
      display: block;
    }

    .card-info {
      padding: 1.2rem 1.3rem;
    }

    .card-title {
      font-size: 1.15rem;
      font-weight: 600;
      margin-bottom: 0.4rem;
    }

    .card-price {
      color: #e44d26;
      font-weight: 700;
      font-size: 1.1rem;
    }

    /* Modal */
    .modal {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.82);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 1000;
      backdrop-filter: blur(4px);
    }

    .modal-content {
      background: white;
      max-width: 520px;
      width: 92%;
      border-radius: 16px;
      overflow: hidden;
      position: relative;
      box-shadow: 0 20px 60px rgba(0,0,0,0.4);
    }

    .modal-img {
      width: 100%;
      height: 340px;
      object-fit: cover;
    }

    .modal-body {
      padding: 1.8rem 2rem 2rem;
    }

    .modal-title {
      font-size: 1.6rem;
      margin-bottom: 1rem;
    }

    .modal-description {
      color: #444;
      margin-bottom: 1.5rem;
      font-size: 1.05rem;
    }

    .modal-price {
      font-size: 1.5rem;
      font-weight: 700;
      color: #e44d26;
      margin-bottom: 1.4rem;
      display: block;
    }

    .close-btn {
      position: absolute;
      top: 14px;
      right: 18px;
      width: 38px;
      height: 38px;
      background: rgba(0,0,0,0.5);
      color: white;
      border: none;
      border-radius: 50%;
      font-size: 1.4rem;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: background 0.2s;
    }

    .close-btn:hover {
      background: rgba(0,0,0,0.75);
    }

    @media (max-width: 600px) {
      .gallery {
        grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
      }
      .modal-img {
        height: 280px;
      }
    }
  </style>
</head>
<body>

<header>
  <h1><strong> <font color="red">welcome to</font> <font color="black">boyz store</font> </strong> </h1>
  <div class="subtitle">Curated pieces • Free shipping over $120</div>
</header>

<div class="container">
  <div class="gallery" id="gallery"></div>
</div>

<!-- Modal -->
<div class="modal" id="modal">
  <div class="modal-content">
    <button class="close-btn" id="close">×</button>
    <img class="modal-img" id="modalImage" src="" alt="">
    <div class="modal-body">
      <h2 class="modal-title" id="modalTitle"></h2>
      <div class="modal-description" id="modalDesc"></div>
      <span class="modal-price" id="modalPrice"></span>
    </div>
  </div>
</div>

<script>
// Sample products (in real project images would be in /images/ folder)
const products = [
  {
    id: 1,
    title: "Hasino ko fasane wala ",
    price: "$189.00",
    image: "one.jpg",
    description: "Premium Italian wool blend in charcoal gray. Fully lined, double-breasted with horn buttons. Perfect for transitional weather."
  },
  {
    id: 2,
    title: "Dhoom-chutad Foji",
    price: "$68.00",
    image: "two.jpg",
    description: "Extra fine merino wool. Tight rib knit for a flattering fit. Available in 7 earthy tones."
  },
  {
    id: 3,
    title: "Curly Tiptiya",
    price: "$248.00",
    image: "three.jpg",
    description: "Full-grain leather from Tuscany. Goodyear welted construction. Signature beige crepe sole."
  },
  {
    id: 4,
    title: "Asman ka parinda - micky darinda",
    price: "$92.00",
    image: "four.jpg",
    description: "100% European flax linen. Garment-dyed for soft hand-feel. Dropped shoulders, oversized fit."
  },
  {
    id: 5,
    title: "Hastmathun Kamlesh",
    price: "$138.00",
    image: "five.jpg",
    description: "Mid-weight wool blend with subtle texture. High-rise, pleated front, pressed creases."
  },
  {
    id: 6,
    title: "Arun Kholi",
    price: "$54.00",
    image: "six.jpg",
    description: "Grade-A Mongolian cashmere. Double-layered fold-up brim. One size fits most."
  }
];

const gallery = document.getElementById('gallery');
const modal = document.getElementById('modal');
const modalImg = document.getElementById('modalImage');
const modalTitle = document.getElementById('modalTitle');
const modalDesc = document.getElementById('modalDesc');
const modalPrice = document.getElementById('modalPrice');
const closeBtn = document.getElementById('close');

// Create cards
products.forEach(product => {
  const card = document.createElement('div');
  card.className = 'card';
  card.innerHTML = `
    <img src="${product.image}" alt="${product.title}">
    
    <div class="card-info">
      <div class="card-title">${product.title}</div>
      <div class="card-price">${product.price}</div>
    </div>
  `;
  
  card.addEventListener('click', () => {
    modalImg.src = product.image;
    
    modalImg.alt = product.title;
    modalTitle.textContent = product.title;
    modalDesc.textContent = product.description;
    modalPrice.textContent = product.price;
    modal.style.display = 'flex';

  });
  
  gallery.appendChild(card);
});

// Close modal
closeBtn.addEventListener('click', () => {
  modal.style.display = 'none';
});

modal.addEventListener('click', e => {
  if (e.target === modal) {
    modal.style.display = 'none';
  }
});

// Close with Escape key
document.addEventListener('keydown', e => {
  if (e.key === 'Escape' && modal.style.display === 'flex') {
    modal.style.display = 'none';
  }
});
</script>
</body>
</html>
