<!doctype html>

<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Mini Shop — Demo</title>
  <meta name="description" content="Demo shopping website (single-file)." />
  <style>
    :root{
      --bg:#0f1724; --card:#0b1220; --accent:#06b6d4; --muted:#94a3b8; --glass: rgba(255,255,255,0.03);
      --radius:12px; --max-width:1100px;
      color-scheme: dark;
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial;color:#e6eef6;background:linear-gradient(180deg,#071021 0%,var(--bg) 100%);}
    .container{max-width:var(--max-width);margin:24px auto;padding:20px}
    header{display:flex;gap:16px;align-items:center;justify-content:space-between}
    .brand{display:flex;gap:12px;align-items:center}
    .logo{width:48px;height:48px;border-radius:10px;background:linear-gradient(135deg,var(--accent),#7c3aed);display:grid;place-items:center;font-weight:700}
    h1{margin:0;font-size:20px}
    .controls{display:flex;gap:10px;align-items:center}
    input[type=search]{background:var(--card);border:1px solid rgba(255,255,255,0.04);padding:10px 12px;border-radius:10px;color:inherit;min-width:200px}
    select{background:var(--card);border:1px solid rgba(255,255,255,0.04);padding:10px;border-radius:10px;color:inherit}
    button{background:linear-gradient(90deg,var(--accent),#7c3aed);border:0;padding:10px 14px;border-radius:10px;color:#042027;font-weight:600;cursor:pointer}
    main{display:grid;grid-template-columns:1fr 320px;gap:20px;margin-top:20px}
    @media (max-width:920px){main{grid-template-columns:1fr} .sidebar{order:2}}/* product grid */
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:16px}
.card{background:linear-gradient(180deg,var(--card),rgba(255,255,255,0.01));padding:12px;border-radius:var(--radius);box-shadow:0 3px 18px rgba(2,6,23,0.6);display:flex;flex-direction:column;gap:10px}
.img{height:160px;border-radius:10px;background-size:cover;background-position:center}
.title{font-weight:700;font-size:15px}
.price{font-weight:800}
.meta{color:var(--muted);font-size:13px}
.actions{display:flex;gap:8px;margin-top:auto;align-items:center}
.btn-ghost{background:transparent;border:1px solid rgba(255,255,255,0.04);padding:8px;border-radius:8px;color:var(--muted);cursor:pointer}

/* sidebar/cart */
.sidebar{background:var(--glass);padding:14px;border-radius:12px;position:sticky;top:24px;height:fit-content}
.cart-item{display:flex;gap:10px;align-items:center;padding:8px 0;border-bottom:1px dashed rgba(255,255,255,0.02)}
.qty{display:flex;gap:6px;align-items:center}
.muted{color:var(--muted)}

footer{margin-top:28px;color:var(--muted);font-size:13px;text-align:center}

/* tiny utilities */
.chip{background:rgba(255,255,255,0.03);padding:6px 8px;border-radius:8px;font-size:13px}
.empty{padding:20px;text-align:center;color:var(--muted)}

  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="brand">
        <div class="logo">MS</div>
        <div>
          <h1>Mini Shop</h1>
          <div class="muted" style="font-size:13px">Demo shopping site — single HTML file</div>
        </div>
      </div><div class="controls">
    <input id="search" type="search" placeholder="Search products, e.g. sneakers" />
    <select id="category">
      <option value="all">All categories</option>
    </select>
    <select id="sort">
      <option value="popular">Sort: Popular</option>
      <option value="price-asc">Price: Low → High</option>
      <option value="price-desc">Price: High → Low</option>
    </select>
    <button id="clearFilters">Clear</button>
    <button id="openCart">Cart (<span id="cartCount">0</span>)</button>
  </div>
</header>

<main>
  <section>
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:12px">
      <div class="muted">Showing <span id="resultCount">0</span> products</div>
      <div class="chip" id="appliedFilters">No filters</div>
    </div>

    <div class="grid" id="productGrid"></div>
    <div id="noResults" class="empty" style="display:none">No products found.</div>
  </section>

  <aside class="sidebar">
    <h3 style="margin:0 0 8px 0">Your Cart</h3>
    <div id="cartList"></div>
    <div style="display:flex;justify-content:space-between;padding-top:10px;font-weight:700">
      <div>Total</div>
      <div id="cartTotal">₹0.00</div>
    </div>
    <div style="margin-top:12px;display:flex;gap:8px">
      <button id="checkout">Checkout</button>
      <button id="clearCart" class="btn-ghost">Clear</button>
    </div>
  </aside>
</main>

<footer>
  This is a demo. For production you should connect a backend, payment gateway, and secure product storage.
</footer>

  </div>  <script>
    // SAMPLE PRODUCTS — replace or extend
    const PRODUCTS = [
      {id:1,name:'Canvas Sneakers',price:799,category:'Footwear',desc:'Comfortable everyday sneakers',img:'https://images.unsplash.com/photo-1600180758890-8f1f3d8a6b7a?w=900&q=80'},
      {id:2,name:'Slim Fit Jeans',price:1299,category:'Clothing',desc:'Stretch denim for comfort',img:'https://images.unsplash.com/photo-1520975698519-1f0b6e48a9b3?w=900&q=80'},
      {id:3,name:'Leather Wallet',price:499,category:'Accessories',desc:'Minimal leather wallet',img:'https://images.unsplash.com/photo-1519744792095-2f2205e87b6f?w=900&q=80'},
      {id:4,name:'Sport Watch',price:2599,category:'Accessories',desc:'Water-resistant sport watch',img:'https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=900&q=80'},
      {id:5,name:'Graphic Tee',price:399,category:'Clothing',desc:'Soft cotton t-shirt with print',img:'https://images.unsplash.com/photo-1520975698519-1f0b6e48a9b3?w=900&q=80'},
      {id:6,name:'Running Shoes',price:1999,category:'Footwear',desc:'Lightweight running shoes',img:'https://images.unsplash.com/photo-1542293787938-c9e299b8800d?w=900&q=80'}
    ];

    // state
    let products = [...PRODUCTS];
    let cart = JSON.parse(localStorage.getItem('mini_shop_cart')||'{}');

    // elements
    const productGrid = document.getElementById('productGrid');
    const cartList = document.getElementById('cartList');
    const cartTotal = document.getElementById('cartTotal');
    const cartCount = document.getElementById('cartCount');
    const search = document.getElementById('search');
    const category = document.getElementById('category');
    const sort = document.getElementById('sort');
    const resultCount = document.getElementById('resultCount');
    const appliedFilters = document.getElementById('appliedFilters');
    const noResults = document.getElementById('noResults');

    function formatINR(n){return '₹'+Number(n).toLocaleString('en-IN',{maximumFractionDigits:2})}

    // render category options
    function populateCategories(){
      const cats = ['All',...new Set(products.map(p=>p.category))];
      category.innerHTML = '';
      cats.forEach(c=>{
        const val = c.toLowerCase();
        const opt = document.createElement('option'); opt.value = val==='all'?'all':c; opt.textContent = c; category.appendChild(opt);
      })
    }

    function renderProducts(list){
      productGrid.innerHTML = '';
      if(list.length===0){noResults.style.display='block'} else {noResults.style.display='none'}
      resultCount.textContent = list.length;
      list.forEach(p=>{
        const card = document.createElement('div'); card.className='card';
        const img = document.createElement('div'); img.className='img'; img.style.backgroundImage = `url(${p.img})`;
        const title = document.createElement('div'); title.className='title'; title.textContent = p.name;
        const desc = document.createElement('div'); desc.className='meta'; desc.textContent = p.desc;
        const price = document.createElement('div'); price.className='price'; price.textContent = formatINR(p.price);
        const actions = document.createElement('div'); actions.className='actions';
        const add = document.createElement('button'); add.textContent='Add to cart'; add.onclick = ()=> addToCart(p.id);
        const view = document.createElement('button'); view.className='btn-ghost'; view.textContent='Quick view'; view.onclick=()=>alert(p.name+' — '+p.desc+'\nPrice: '+formatINR(p.price));
        actions.appendChild(add); actions.appendChild(view);
        card.appendChild(img); card.appendChild(title); card.appendChild(desc); card.appendChild(price); card.appendChild(actions);
        productGrid.appendChild(card);
      })
    }

    function addToCart(id){
      const key = String(id);
      cart[key] = (cart[key]||0)+1;
      saveCart();
      renderCart();
    }
    function removeFromCart(id){
      const key = String(id);
      delete cart[key]; saveCart(); renderCart();
    }
    function changeQty(id,delta){
      const key = String(id);
      cart[key] = Math.max(0,(cart[key]||0)+delta);
      if(cart[key]===0) delete cart[key];
      saveCart(); renderCart();
    }

    function saveCart(){
      localStorage.setItem('mini_shop_cart',JSON.stringify(cart));
    }

    function renderCart(){
      cartList.innerHTML='';
      let total = 0; let count = 0;
      const ids = Object.keys(cart);
      if(ids.length===0){ cartList.innerHTML = '<div class="empty">Cart is empty</div>'; }
      ids.forEach(k=>{
        const qty = cart[k];
        const prod = products.find(p=>String(p.id)===k);
        if(!prod) return;
        count += qty; total += qty*prod.price;
        const row = document.createElement('div'); row.className='cart-item';
        row.innerHTML = `
          <div style="width:48px;height:48px;border-radius:8px;background-image:url(${prod.img});background-size:cover;background-position:center"></div>
          <div style="flex:1">
            <div style="font-weight:700">${prod.name}</div>
            <div class="muted">${formatINR(prod.price)} × ${qty}</div>
          </div>
        `;
        const controls = document.createElement('div'); controls.style.textAlign='right';
        const qtybox = document.createElement('div'); qtybox.className='qty';
        const minus = document.createElement('button'); minus.className='btn-ghost'; minus.textContent='−'; minus.onclick=()=>changeQty(prod.id,-1);
        const plus = document.createElement('button'); plus.className='btn-ghost'; plus.textContent='+'; plus.onclick=()=>changeQty(prod.id,1);
        const del = document.createElement('button'); del.className='btn-ghost'; del.textContent='Delete'; del.onclick=()=>removeFromCart(prod.id);
        qtybox.appendChild(minus); qtybox.appendChild(plus); qtybox.appendChild(del);
        controls.appendChild(qtybox);
        row.appendChild(controls);
        cartList.appendChild(row);
      });
      cartTotal.textContent = formatINR(total);
      cartCount.textContent = count;
    }

    // filters
    function applyFilters(){
      const q = search.value.trim().toLowerCase();
      const cat = category.value;
      const s = sort.value;
      let out = products.filter(p=>{
        const matchesQ = !q || (p.name+p.desc+p.category).toLowerCase().includes(q);
        const matchesCat = cat==='all' || p.category===cat;
        return matchesQ && matchesCat;
      });
      if(s==='price-asc') out.sort((a,b)=>a.price-b.price);
      else if(s==='price-desc') out.sort((a,b)=>b.price-a.price);
      else out = out; // popular (original order)

      appliedFilters.textContent = (q?('"'+q+'" '):'') + (cat!=='all'?(' • '+cat):'') + (s!=='popular'?(' • '+s):'') || 'No filters';
      renderProducts(out);
    }

    // events
    document.getElementById('clearFilters').onclick = ()=>{search.value='';category.value='all';sort.value='popular';applyFilters()}
    document.getElementById('openCart').onclick = ()=>{window.scrollTo({top:document.body.scrollHeight,behavior:'smooth'})}
    document.getElementById('checkout').onclick = ()=>{
      if(Object.keys(cart).length===0){alert('Cart empty — add something first')} else {alert('Checkout demo: total '+cartTotal.textContent+' — integrate a payment gateway for real checkout.');}
    }
    document.getElementById('clearCart').onclick = ()=>{if(confirm('Clear cart?')){cart={};saveCart();renderCart();}}

    [search,category,sort].forEach(el=>el.addEventListener('input',applyFilters));

    // init
    function init(){
      populateCategories();
      renderProducts(products);
      renderCart();
      applyFilters();
    }
    init();

    // expose products so user can edit from console if desired
    window.MINI_SHOP = {products,cart,addToCart,renderCart};
  </script></body>
</html>
