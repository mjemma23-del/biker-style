
[bikerstyle_store (2).html](https://github.com/user-attachments/files/22185014/bikerstyle_store.2.html)
<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>BikerStyle — Tienda Online</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#fff;color:#000}
header{background:#f2f2f2;display:flex;align-items:center;justify-content:space-between;padding:12px 24px;position:sticky;top:0;z-index:10}
header .brand{font-weight:bold;font-size:20px;display:flex;align-items:center;gap:10px}
header nav{display:flex;gap:16px}
header nav a{color:#000;text-decoration:none;font-weight:500}
header nav a:hover{text-decoration:underline}
#carousel{position:relative;overflow:hidden;margin:16px auto;border-radius:12px;max-width:1200px;height:300px}
#carousel img{width:100%;height:100%;object-fit:cover;position:absolute;top:0;left:0;opacity:0;transition:opacity 1s}
#carousel img.active{opacity:1}
.carousel-controls{position:absolute;top:50%;width:100%;display:flex;justify-content:space-between;transform:translateY(-50%)}
.carousel-controls button{background:rgba(0,0,0,0.2);border:none;color:#000;padding:8px 12px;border-radius:6px;cursor:pointer}
main{max-width:1200px;margin:16px auto;padding:0 12px}
.products-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:16px}
.product-card{background:#f9f9f9;padding:12px;border-radius:10px;border:1px solid rgba(0,0,0,0.05);display:flex;flex-direction:column;color:#000}
.product-card img{width:100%;height:160px;object-fit:cover;border-radius:8px}
.product-card h3{margin:8px 0 4px;font-size:16px}
.product-card .price{font-weight:bold}
.product-card .badge{background:#ff9800;color:#fff;padding:2px 6px;border-radius:6px;font-size:12px;margin-top:4px;display:inline-block}
.product-card button{margin-top:auto;padding:8px 12px;background:#ff9800;border:none;border-radius:8px;color:#fff;font-weight:600;cursor:pointer}
#cartPanel{position:fixed;right:16px;top:80px;width:360px;background:#f9f9f9;padding:14px;border-radius:12px;border:1px solid rgba(0,0,0,0.05);display:none;z-index:15;color:#000}
#cartPanel h2{margin-top:0}
.cart-item{display:flex;justify-content:space-between;align-items:center;padding:6px 0;border-bottom:1px solid rgba(0,0,0,0.05)}
.cart-item img{width:50px;height:50px;object-fit:cover;border-radius:6px;margin-right:8px}
.cart-controls button{background:#ff9800;border:none;color:#fff;padding:4px 8px;border-radius:6px;cursor:pointer}
#dashboardModal{position:fixed;inset:0;background:rgba(0,0,0,0.3);display:none;align-items:center;justify-content:center;z-index:20}
#dashboardModal .modal-content{background:#f9f9f9;padding:16px;border-radius:12px;width:900px;max-width:95vw;max-height:90vh;overflow:auto;color:#000}
#dashboardModal h2{margin-top:0}
#dashboardModal input, #dashboardModal select{width:100%;padding:8px;margin-bottom:8px;border-radius:6px;background:#fff;color:#000;border:1px solid rgba(0,0,0,0.1)}
#dashboardModal button{padding:8px 12px;border:none;border-radius:6px;background:#ff9800;color:#fff;font-weight:600;cursor:pointer;margin-right:6px}
footer{text-align:center;padding:12px;background:#f2f2f2;margin-top:16px;color:#000;font-size:14px}
@media(max-width:768px){.products-grid{grid-template-columns:1fr 1fr}}
@media(max-width:480px){.products-grid{grid-template-columns:1fr}}
</style>
</head>
<body>
<header>
<div class="brand">BikerStyle</div>
<nav>
<a href="#">Cascos</a>
<a href="#">Chaquetas</a>
<a href="#">Guantes</a>
<a href="#">Botas</a>
<a href="#">Accesorios</a>
<a href="#">Outlet</a>
<a href="#" id="openDashboardBtn">Dashboard</a>
<a href="#" id="openCartBtn">Carrito</a>
</nav>
</header>
<div id="carousel">
<img src="https://images.unsplash.com/photo-1622187180508-5d9aeb7a6f88?auto=format&fit=crop&w=1200&q=80" class="active">
<img src="https://images.unsplash.com/photo-1600371230197-5935b36408bb?auto=format&fit=crop&w=1200&q=80">
<img src="https://images.unsplash.com/photo-1599861109574-70d5c3f07f9a?auto=format&fit=crop&w=1200&q=80">
<div class="carousel-controls">
<button id="prevSlide">&#10094;</button>
<button id="nextSlide">&#10095;</button>
</div>
</div>
<main>
<div class="products-grid" id="productsGrid"></div>
</main>
<div id="cartPanel">
<h2>Carrito</h2>
<div id="cartItems"></div>
<p><strong>Total: $<span id="cartTotal">0</span></strong></p>
<button id="checkoutBtn">Finalizar compra</button>
<button id="closeCartBtn">Cerrar</button>
</div>
<div id="dashboardModal">
<div class="modal-content">
<h2>Dashboard — BikerStyle</h2>
<input type="password" id="dashPassword" placeholder="Contraseña">
<button id="loginDashBtn">Entrar</button>
<button id="closeDashBtn">Cerrar</button>
<hr>
<div id="adminArea" style="display:none">
<h3>Administrar Productos</h3>
<input type="text" id="pName" placeholder="Nombre del producto">
<input type="number" id="pPrice" placeholder="Precio">
<input type="text" id="pImg" placeholder="URL imagen">
<input type="text" id="pBadge" placeholder="Badge">
<button id="addProductBtn">Agregar producto</button>
<div id="adminProducts"></div>
</div>
</div>
</div>
<footer>© 2025 BikerStyle — Tienda Demo · Contraseña dashboard: <strong>admin123</strong></footer>
<script>
// Carousel
let slides=document.querySelectorAll('#carousel img');let current=0;function showSlide(n){slides.forEach((s,i)=>s.classList.remove('active'));slides[n].classList.add('active')}function nextSlide(){current=(current+1)%slides.length;showSlide(current)}function prevSlide(){current=(current-1+slides.length)%slides.length;showSlide(current)}document.getElementById('nextSlide').onclick=nextSlide;document.getElementById('prevSlide').onclick=prevSlide;setInterval(nextSlide,5000);

// Products
let products=[{name:'Casco Integral AXOR Apex',price:1899,img:'https://images.unsplash.com/photo-1618322769096-69e5f1f1f167?auto=format&fit=crop&w=600&q=80',badge:'Nuevo'},{name:'Chamarra REV\'IT! Eclipse',price:3299,img:'https://images.unsplash.com/photo-1600371230197-5935b36408bb?auto=format&fit=crop&w=600&q=80'},{name:'Guantes Alpinestars SP-8',price:1499,img:'https://images.unsplash.com/photo-1606865590473-7fcf5a6b57c3?auto=format&fit=crop&w=600&q=80'},{name:'Botas Dainese Sport Master',price:4999,img:'https://images.unsplash.com/photo-1600371229990-fb7b8c34ff7f?auto=format&fit=crop&w=600&q=80'},{name:'Casco Shoei GT-Air II',price:2999,img:'https://images.unsplash.com/photo-1618322769100-4f3f5e0d1aa4?auto=format&fit=crop&w=600&q=80',badge:'Oferta'}];

let cart=[];function renderProducts(){let grid=document.getElementById('productsGrid');grid.innerHTML=products.map((p,i)=>`<div class="product-card"><img src="${p.img}"><h3>${p.name}</h3><div class="price">$${p.price}</div>${p.badge?'<div class="badge">'+p.badge+'</div>':''}<button onclick="addToCart(${i})">Agregar</button></div>`).join('')} 
function addToCart(i){let p=products[i];let existing=cart.find(c=>c.name===p.name);if(existing){existing.qty++;}else{cart.push({name:p.name,price:p.price,img:p.img,qty:1})}renderCart()}
function renderCart(){let el=document.getElementById('cartItems');if(cart.length===0){el.innerHTML='<p>Carrito vacío</p>';}else{el.innerHTML=cart.map(c=>`<div class="cart-item"><img src="${c.img}"><div>${c.name} x ${c.qty} <span>$${c.price*c.qty}</span></div><div class="cart-controls"><button onclick="removeFromCart('${c.name}')">Eliminar</button></div></div>`).join('');}document.getElementById('cartTotal').textContent=cart.reduce((a,b)=>a+b.price*b.qty,0)}
function removeFromCart(name){cart=cart.filter(c=>c.name!==name);renderCart()}

// Cart toggle
document.getElementById('openCartBtn').onclick=()=>{document.getElementById('cartPanel').style.display='block'}
document.getElementById('closeCartBtn').onclick=()=>{document.getElementById('cartPanel').style.display='none'}
document.getElementById('checkoutBtn').onclick=()=>{if(cart.length===0){alert('Carrito vacío');return;}alert('Compra simulada. Total: $'+cart.reduce((a,b)=>a+b.price*b.qty,0));cart=[];renderCart();document.getElementById('cartPanel').style.display='none'}

// Dashboard
document.getElementById('openDashboardBtn').onclick=()=>{document.getElementById('dashboardModal').style.display='flex'}
document.getElementById('closeDashBtn').onclick=()=>{document.getElementById('dashboardModal').style.display='none';document.getElementById('adminArea').style.display='none'}
document.getElementById('loginDashBtn').onclick=()=>{if(document.getElementById('dashPassword').value==='admin123'){document.getElementById('adminArea').style.display='block';alert('Bienvenido al dashboard');}else{alert('Contraseña incorrecta')}}
document.getElementById('addProductBtn').onclick=()=>{let n=document.getElementById('pName').value,p=document.getElementById('pPrice').value,i=document.getElementById('pImg').value,b=document.getElementById('pBadge').value;products.push({name:n,price:parseFloat(p),img:i,badge:b});renderProducts()}

renderProducts();
</script>
</body>
</html>
