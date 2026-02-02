<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SKY STORE | JORDAN V27</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;900&display=swap');
        
        :root { --primary: #000; --accent: #ff0000; --bg: #ffffff; }
        body { font-family: 'Cairo', sans-serif; background: #fafafa; overflow-x: hidden; scroll-behavior: smooth; color: #111; margin: 0; padding: 0; }
        
        .product-card { transition: all 0.5s ease; border: 1px solid #eee; border-radius: 2rem; background: #fff; overflow: hidden; display: flex; flex-direction: column; }
        .product-card:hover { transform: translateY(-10px); box-shadow: 0 20px 40px rgba(0,0,0,0.08); border-color: #000; }
        
        .admin-sidebar, .cart-sidebar { transition: transform 0.6s cubic-bezier(0.8, 0, 0.2, 1); z-index: 5000; position: fixed; top: 0; right: 0; height: 100%; background: #fff; width: 100%; max-width: 500px; box-shadow: -10px 0 40px rgba(0,0,0,0.1); }
        .sidebar-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.4); z-index: 4500; backdrop-filter: blur(8px); }
        
        .primary-btn { background: #000; color: #fff; transition: 0.3s; border-radius: 15px; font-weight: 700; border: 2px solid #000; cursor: pointer; text-align: center; display: block; width: 100%; padding: 15px; }
        .primary-btn:hover { background: transparent; color: #000; }
        
        #product-modal, #checkout-modal, #success-modal { display: none; z-index: 6000; position: fixed; inset: 0; align-items: center; justify-content: center; background: rgba(0,0,0,0.5); backdrop-filter: blur(10px); padding: 20px; }
        .modal-content { background: #fff; border-radius: 2.5rem; width: 100%; max-width: 1000px; max-height: 90vh; overflow-y: auto; position: relative; }

        .news-ticker { background: #000; color: #fff; padding: 14px 0; overflow: hidden; white-space: nowrap; position: fixed; bottom: 0; width: 100%; z-index: 3000; border-top: 3px solid #ff0000; }
        .ticker-text { display: inline-block; animation: ticker 35s linear infinite; font-weight: 800; font-size: 14px; }
        @keyframes ticker { from { transform: translateX(100%); } to { transform: translateX(-100%); } }

        .media-viewer { width: 100%; height: 400px; display: flex; align-items: center; justify-content: center; background: #fdfdfd; border-radius: 2rem; overflow: hidden; border: 1px solid #eee; }
        .media-viewer img, .media-viewer video { max-width: 100%; max-height: 100%; object-fit: contain; }
        
        .thumb-nav { width: 65px; height: 65px; object-fit: cover; border-radius: 12px; cursor: pointer; border: 2px solid transparent; opacity: 0.6; transition: 0.3s; }
        .thumb-active { border-color: #000; opacity: 1; transform: scale(1.1); }

        .admin-input { width: 100%; border: 1.5px solid #eee; padding: 16px; border-radius: 15px; outline: none; transition: 0.3s; background: #f9f9f9; font-weight: 600; }
        .admin-input:focus { border-color: #000; background: #fff; box-shadow: 0 0 10px rgba(0,0,0,0.05); }

        .order-card { background: #fff; border-right: 6px solid #000; padding: 20px; border-radius: 15px; margin-bottom: 15px; box-shadow: 0 5px 15px rgba(0,0,0,0.03); border: 1px solid #eee; border-right-width: 6px; }
        .comment-box { border-bottom: 1px solid #eee; padding: 15px 0; }
    </style>
</head>
<body>

    <div id="sidebar-overlay" class="sidebar-overlay" onclick="closeAllSidebars()"></div>

    <nav class="sticky top-0 bg-white/90 backdrop-blur-lg shadow-sm z-[1000] border-b">
        <div class="container mx-auto px-6 md:px-12 py-5 flex justify-between items-center">
            <div class="text-3xl font-black tracking-tighter cursor-pointer" onclick="location.reload()">
                SKY <span class="text-red-600">STORE</span>
            </div>
            <div class="flex items-center gap-8">
                <div class="relative hidden lg:block">
                    <input type="text" id="global-search" onkeyup="searchEngine()" placeholder="ابحث عن منتج..." class="admin-input !py-2 !px-8 !w-64 !rounded-full text-xs">
                </div>
                <div class="flex items-center gap-6 text-2xl">
                    <i class="fas fa-cog cursor-pointer hover:text-red-600 transition" onclick="toggleAdmin()"></i>
                    <div class="relative cursor-pointer" onclick="toggleCart()">
                        <i class="fas fa-shopping-bag"></i>
                        <span id="cart-counter" class="absolute -top-2 -right-2 bg-red-600 text-white text-[10px] rounded-full w-5 h-5 flex items-center justify-center font-bold">0</span>
                    </div>
                </div>
            </div>
        </div>
    </nav>

    <div class="bg-white border-b sticky top-[80px] z-[900] py-4 overflow-x-auto no-scrollbar">
        <div class="container mx-auto px-4 flex gap-8 justify-center min-w-max" id="category-list"></div>
    </div>

    <section class="container mx-auto px-6 py-8">
        <div class="relative rounded-[3rem] overflow-hidden h-[350px] md:h-[500px] shadow-2xl">
            <img src="https://images.unsplash.com/photo-1441984904996-e0b6ba687e04?w=1600" class="w-full h-full object-cover">
            <div class="absolute inset-0 bg-gradient-to-t from-black/70 via-black/20 to-transparent flex flex-col justify-end p-12 text-white">
                <h2 class="text-5xl md:text-8xl font-black mb-4 italic tracking-tighter">Happy New Year</h2>
                <p class="text-xl font-bold mb-8 text-gray-200">أقوى العروض الحصرية في الأردن 🇯🇴</p>
                <button class="bg-white text-black px-12 py-4 font-black rounded-2xl w-fit hover:bg-red-600 hover:text-white transition-all">اكتشف الآن</button>
            </div>
        </div>
    </section>

    <main class="container mx-auto px-6 py-12">
        <h3 class="text-4xl font-black mb-16 text-center italic underline decoration-red-600 underline-offset-8">المجموعة الجديدة</h3>
        <div id="product-grid" class="grid grid-cols-2 md:grid-cols-3 xl:grid-cols-4 gap-10"></div>
    </main>

    <div id="product-modal">
        <div class="modal-content flex flex-col lg:flex-row shadow-2xl">
            <button onclick="closeModal()" class="absolute top-6 left-6 w-12 h-12 bg-white rounded-full shadow-lg font-black z-50 hover:bg-red-600 hover:text-white transition">×</button>
            <div class="lg:w-1/2 p-8 bg-[#fcfcfc]">
                <div id="media-viewer" class="media-viewer mb-6 shadow-inner"></div>
                <div id="modal-gallery" class="flex gap-3 overflow-x-auto py-2 no-scrollbar"></div>
                
                <div class="mt-8 p-6 bg-white rounded-3xl border">
                    <h4 class="font-black mb-4 flex items-center gap-2"><i class="fas fa-comments text-red-600"></i> آراء الناس</h4>
                    <div id="comments-list" class="max-h-60 overflow-y-auto mb-4 space-y-4"></div>
                    <div class="space-y-3 pt-4 border-t">
                        <input id="rev-name" placeholder="اسمك" class="admin-input !py-2 text-sm">
                        <textarea id="rev-text" placeholder="شو رأيك بالمنتج؟" class="admin-input !py-2 text-sm h-20"></textarea>
                        <button onclick="addComment()" class="primary-btn !py-2 text-sm">نشر التعليق</button>
                    </div>
                </div>
            </div>
            <div class="lg:w-1/2 p-12 flex flex-col justify-center">
                <span id="modal-cat-tag" class="text-xs font-black text-red-600 mb-4 block uppercase tracking-[0.2em]"></span>
                <h2 id="modal-name" class="text-4xl font-black mb-6 tracking-tight"></h2>
                <p id="modal-price" class="text-4xl font-black mb-8 text-black italic"></p>
                <div class="mb-10 p-8 bg-gray-50 rounded-[2rem] border-r-8 border-black shadow-sm">
                    <p id="modal-desc" class="text-gray-600 leading-relaxed font-bold italic"></p>
                </div>
                <button id="modal-add-btn" class="primary-btn !py-5 text-xl shadow-xl shadow-black/10">إضافة إلى السلة</button>
            </div>
        </div>
    </div>

    <div id="checkout-modal">
        <div class="modal-content !max-w-md p-12 shadow-2xl border-[6px] border-black">
            <h2 class="text-3xl font-black mb-10 text-center italic">تأكيد طلبك</h2>
            <div class="space-y-6">
                <input type="text" id="cust-name" placeholder="الاسم بالكامل" class="admin-input">
                <input type="tel" id="cust-phone" placeholder="رقم الجوال الأردني" class="admin-input">
                <textarea id="cust-address" placeholder="العنوان بالتفصيل (ملكا، حاتم، المنصورة...)" class="admin-input h-28"></textarea>
                <button onclick="confirmFinalOrder()" class="primary-btn !py-5 text-xl">إرسال الطلب الآن</button>
                <button onclick="closeCheckout()" class="text-center w-full font-bold text-gray-400">إلغاء</button>
            </div>
        </div>
    </div>

    <div id="success-modal">
        <div class="modal-content !max-w-sm p-12 text-center border-t-8 border-green-500">
            <div class="w-24 h-24 bg-green-500 text-white rounded-full flex items-center justify-center text-5xl mx-auto mb-8 shadow-lg">✓</div>
            <h2 class="text-3xl font-black mb-4 tracking-tighter">تم بنجاح!</h2>
            <p class="text-gray-500 font-bold mb-10">وصلنا طلبك ، رح نتصل فيك بأقرب وقت لتأكيد الموعد.</p>
            <button onclick="location.reload()" class="bg-black text-white px-12 py-4 rounded-2xl font-black shadow-xl">موافق</button>
        </div>
    </div>

    <div id="admin-sidebar" class="admin-sidebar translate-x-full overflow-y-auto">
        <div class="p-10">
            <div class="flex justify-between items-center mb-12">
                <h2 class="text-4xl font-black italic">CONTROL</h2>
                <button onclick="closeAllSidebars()" class="text-3xl font-light">×</button>
            </div>
            
            <div class="flex gap-4 mb-10">
                <button onclick="switchAdminTab('products')" id="tab-p" class="flex-1 py-4 bg-black text-white font-black rounded-2xl text-sm transition shadow-lg shadow-black/20">المنتجات</button>
                <button onclick="switchAdminTab('orders')" id="tab-o" class="flex-1 py-4 bg-gray-100 text-black font-black rounded-2xl text-sm transition">الطلبات الواردة</button>
            </div>

            <div id="admin-products-sec">
                <div class="space-y-4 bg-gray-50 p-8 rounded-[2.5rem] mb-12 border border-white shadow-inner">
                    <input id="p-name" placeholder="اسم المنتج" class="admin-input">
                    <div class="grid grid-cols-2 gap-4">
                        <input id="p-price" placeholder="السعر (JD)" type="text" class="admin-input">
                        <select id="p-cat" class="admin-input"></select>
                    </div>
                    <textarea id="p-desc" placeholder="وصف المنتج..." class="admin-input h-28"></textarea>
                    <input id="p-img" placeholder="رابط الصورة الرئيسية" class="admin-input">
                    <input id="p-imgs" placeholder="8 روابط صور إضافية (افصل بفاصلة ,)" class="admin-input text-xs">
                    <input id="p-video" placeholder="رابط فيديو المنتج (MP4)" class="admin-input text-xs">
                    <input type="hidden" id="edit-id">
                    <button id="save-btn" onclick="saveProduct()" class="primary-btn !bg-red-600 !border-red-600 mt-4 shadow-xl shadow-red-600/20">حفظ في المتجر</button>
                </div>
                <div id="admin-stock" class="space-y-4 pb-24"></div>
            </div>

            <div id="admin-orders-sec" class="hidden">
                <div id="orders-list" class="space-y-4 pb-24"></div>
            </div>
        </div>
    </div>

    <div id="cart-sidebar" class="cart-sidebar translate-x-full flex flex-col">
        <div class="p-10 bg-black text-white flex justify-between items-center">
            <h2 class="text-3xl font-black italic tracking-widest">MY BAG</h2>
            <button onclick="closeAllSidebars()" class="text-3xl">×</button>
        </div>
        <div id="cart-items" class="flex-grow p-8 overflow-y-auto bg-[#fdfdfd]"></div>
        <div class="p-10 border-t bg-white">
            <div class="flex justify-between items-center mb-8">
                <span class="text-gray-400 font-bold uppercase tracking-widest">Total Price</span>
                <span id="cart-total" class="text-4xl font-black">0.00 JD</span>
            </div>
            <button onclick="openCheckout()" class="primary-btn !py-6 text-2xl shadow-2xl">إتمام الطلب</button>
        </div>
    </div>

    <div class="news-ticker">
        <div class="ticker-text">
             • عرض لفتره محدوده التوصيل لجميع مناطق ملكا والمنصوره وحاتم وام قيس وما حولها مجاناااا لفتره محدود • 
        </div>
    </div>

    <script>
        let products = JSON.parse(localStorage.getItem('sky_v27_db')) || [
            { id: 1, name: "جاكيت شتوي ملكي", price: "25.00", category: "رجالي", desc: "خامة أصلية 100%.", img: "https://images.unsplash.com/photo-1591047139829-d91aecb6caea?w=600", imgs: [], video: [], comments: [] }
        ];
        let ordersArchive = JSON.parse(localStorage.getItem('sky_v27_orders')) || [];
        let categories = ["الكل", "رجالي", "نسائي", "أحذية", "إلكترونيات", "إكسسوارات"];
        let cart = [];
        let activeCat = "الكل";
        let currentProductId = null;

        function bootApp() {
            renderCategories(); renderProducts(products); renderAdminStock(); updateCart(); renderOrders();
            localStorage.setItem('sky_v27_db', JSON.stringify(products));
            localStorage.setItem('sky_v27_orders', JSON.stringify(ordersArchive));
        }

        function renderCategories() {
            const list = document.getElementById('category-list');
            list.innerHTML = categories.map(c => `
                <button onclick="filterBy('${c}')" class="text-[12px] font-black uppercase tracking-[0.2em] transition-all ${activeCat === c ? 'text-red-600 border-b-2 border-red-600 pb-1' : 'text-gray-400 hover:text-black'}">
                    ${c}
                </button>
            `).join('');
            document.getElementById('p-cat').innerHTML = categories.filter(c => c !== "الكل").map(c => `<option value="${c}">${c}</option>`).join('');
        }

        function renderProducts(items) {
            const grid = document.getElementById('product-grid');
            const filtered = activeCat === "الكل" ? items : items.filter(p => p.category === activeCat);
            grid.innerHTML = filtered.map(p => `
                <div class="product-card group" onclick="openProduct(${p.id})">
                    <div class="aspect-[3/4] overflow-hidden bg-gray-100">
                        <img src="${p.img}" class="w-full h-full object-cover group-hover:scale-110 transition duration-700">
                    </div>
                    <div class="p-8 text-right">
                        <h3 class="text-sm font-black truncate mb-3 text-gray-800 uppercase">${p.name}</h3>
                        <p class="font-black text-2xl italic text-black">${p.price} JD</p>
                    </div>
                </div>
            `).join('');
        }

        function openProduct(id) {
            const p = products.find(x => x.id === id);
            currentProductId = id;
            document.getElementById('modal-name').innerText = p.name;
            document.getElementById('modal-price').innerText = p.price + " JD";
            document.getElementById('modal-desc').innerText = p.desc || "فخامة وجودة SKY STORE.";
            document.getElementById('modal-cat-tag').innerText = p.category;
            
            const viewer = document.getElementById('media-viewer');
            const gal = document.getElementById('modal-gallery');
            
            viewer.innerHTML = `<img src="${p.img}" class="fade-in">`;
            let galHTML = `<img src="${p.img}" class="thumb-nav thumb-active" onclick="switchMedia('img', '${p.img}', this)">`;
            
            if(p.imgs) p.imgs.forEach(url => { if(url) galHTML += `<img src="${url.trim()}" class="thumb-nav" onclick="switchMedia('img', '${url.trim()}', this)">`; });
            if(p.video) galHTML += `<div class="thumb-nav bg-black flex items-center justify-center text-white text-[10px]" onclick="switchMedia('video', '${p.video}', this)">VIDEO</div>`;
            
            gal.innerHTML = galHTML;
            renderComments(p.comments || []);
            document.getElementById('modal-add-btn').onclick = () => { cart.push(p); updateCart(); closeModal(); toggleCart(); };
            document.getElementById('product-modal').style.display = 'flex';
        }

        function renderComments(comments) {
            const list = document.getElementById('comments-list');
            if(!comments || comments.length === 0) {
                list.innerHTML = `<p class="text-xs text-gray-400 italic">لا توجد تعليقات بعد، كن أول من يعلق!</p>`;
                return;
            }
            list.innerHTML = comments.map(c => `
                <div class="comment-box text-right">
                    <div class="flex justify-between items-center mb-1">
                        <span class="font-black text-xs">${c.name}</span>
                        <span class="text-[10px] text-gray-400">${c.date}</span>
                    </div>
                    <p class="text-xs text-gray-600 font-bold">${c.text}</p>
                </div>
            `).join('');
        }

        function addComment() {
            const name = document.getElementById('rev-name').value;
            const text = document.getElementById('rev-text').value;
            if(!name || !text) return alert("اكتب اسمك وتعليقك يا غالي");
            
            const pIndex = products.findIndex(x => x.id === currentProductId);
            if(!products[pIndex].comments) products[pIndex].comments = [];
            
            products[pIndex].comments.unshift({
                name, text, date: new Date().toLocaleDateString('ar-JO')
            });
            
            localStorage.setItem('sky_v27_db', JSON.stringify(products));
            renderComments(products[pIndex].comments);
            document.getElementById('rev-name').value = '';
            document.getElementById('rev-text').value = '';
        }

        function switchMedia(type, url, el) {
            const viewer = document.getElementById('media-viewer');
            document.querySelectorAll('.thumb-nav').forEach(t => t.classList.remove('thumb-active'));
            el.classList.add('thumb-active');
            if(type === 'img') viewer.innerHTML = `<img src="${url}">`;
            else viewer.innerHTML = `<video src="${url}" controls autoplay class="w-full h-full"></video>`;
        }

        function updateCart() {
            document.getElementById('cart-counter').innerText = cart.length;
            const total = cart.reduce((s, i) => s + parseFloat(i.price), 0);
            document.getElementById('cart-total').innerText = total.toFixed(2) + " JD";
            document.getElementById('cart-items').innerHTML = cart.map((item, idx) => `
                <div class="flex gap-6 items-center bg-white p-6 rounded-[1.5rem] border mb-4 shadow-sm">
                    <img src="${item.img}" class="w-20 h-24 object-cover rounded-xl">
                    <div class="flex-1">
                        <h5 class="text-xs font-black uppercase mb-1">${item.name}</h5>
                        <p class="text-red-600 font-black text-lg">${item.price} JD</p>
                        <button onclick="cart.splice(${idx}, 1); updateCart()" class="text-[10px] text-gray-400 underline mt-2 hover:text-black">REMOVE</button>
                    </div>
                </div>
            `).join('');
        }

        function saveProduct() {
            const id = document.getElementById('edit-id').value;
            const name = document.getElementById('p-name').value;
            const price = document.getElementById('p-price').value; 
            const img = document.getElementById('p-img').value;
            
            if(!name || !price || !img) return alert("أكمل البيانات المطلوبة!");

            const productData = {
                id: id ? Number(id) : Date.now(),
                name, price, 
                category: document.getElementById('p-cat').value,
                desc: document.getElementById('p-desc').value,
                img,
                imgs: document.getElementById('p-imgs').value.split(',').filter(x => x.trim()),
                video: document.getElementById('p-video').value,
                comments: id ? (products.find(x => x.id == id).comments || []) : []
            };

            if(id) products = products.map(p => p.id == id ? productData : p);
            else products.unshift(productData);

            document.getElementById('p-name').value = ''; document.getElementById('p-price').value = ''; document.getElementById('edit-id').value = '';
            bootApp(); closeAllSidebars();
        }

        function confirmFinalOrder() {
            const n = document.getElementById('cust-name').value;
            const p = document.getElementById('cust-phone').value;
            const a = document.getElementById('cust-address').value;
            if(!n || !p || !a) return alert("يرجى تعبئة كافة البيانات!");

            ordersArchive.unshift({
                id: Date.now(), customer: n, phone: p, address: a,
                items: cart.map(i => i.name).join(', '),
                total: cart.reduce((s, i) => s + parseFloat(i.price), 0),
                date: new Date().toLocaleString('ar-JO')
            });

            localStorage.setItem('sky_v27_orders', JSON.stringify(ordersArchive));
            cart = []; updateCart(); closeCheckout(); document.getElementById('success-modal').style.display = 'flex';
        }

        function renderOrders() {
            const list = document.getElementById('orders-list');
            if(ordersArchive.length === 0) { list.innerHTML = `<p class="text-center py-20 text-gray-400 font-bold">لا يوجد طلبات بعد</p>`; return; }
            list.innerHTML = ordersArchive.map(o => `
                <div class="order-card text-right">
                    <div class="flex justify-between items-center mb-4">
                        <span class="text-[10px] bg-black text-white px-3 py-1 rounded-full font-bold">${o.date}</span>
                        <span class="font-black text-red-600 text-lg">${o.total} JD</span>
                    </div>
                    <h4 class="font-black text-lg mb-1">${o.customer}</h4>
                    <p class="text-sm text-gray-500 mb-4">${o.phone} | ${o.address}</p>
                    <div class="bg-gray-50 p-4 rounded-xl text-xs font-bold leading-relaxed border border-gray-100">
                        المنتجات: ${o.items}
                    </div>
                </div>
            `).join('');
        }

        function renderAdminStock() {
            document.getElementById('admin-stock').innerHTML = products.map(p => `
                <div class="flex items-center justify-between bg-white p-5 rounded-[1.5rem] border shadow-sm">
                    <div class="flex items-center gap-4">
                        <img src="${p.img}" class="w-12 h-16 object-cover rounded-lg shadow-sm">
                        <div class="text-right">
                            <h5 class="text-xs font-black">${p.name}</h5>
                            <p class="text-red-600 font-black text-sm">${p.price} JD</p>
                        </div>
                    </div>
                    <div class="flex gap-4">
                        <i onclick="editProduct(${p.id})" class="fas fa-edit text-blue-500 cursor-pointer text-xl"></i>
                        <i onclick="if(confirm('حذف؟')){products=products.filter(x=>x.id!=${p.id});bootApp()}" class="fas fa-trash text-red-500 cursor-pointer text-xl"></i>
                    </div>
                </div>
            `).join('');
        }

        function editProduct(id) {
            const p = products.find(x => x.id == id);
            document.getElementById('edit-id').value = p.id;
            document.getElementById('p-name').value = p.name;
            document.getElementById('p-price').value = p.price;
            document.getElementById('p-img').value = p.img;
            document.getElementById('p-imgs').value = p.imgs ? p.imgs.join(',') : '';
            document.getElementById('p-video').value = p.video || '';
            document.getElementById('p-desc').value = p.desc || '';
            document.getElementById('save-btn').innerText = "تحديث البيانات";
            window.scrollTo({top: 0, behavior: 'smooth'});
        }

        function toggleAdmin() { 
            const el = document.getElementById('admin-sidebar'); 
            if(el.classList.contains('translate-x-full')) { 
                if(prompt("كلمة السر:") === "panzer1234@!!") { 
                    el.classList.remove('translate-x-full'); 
                    document.getElementById('sidebar-overlay').style.display = 'block'; 
                } 
            } else { 
                closeAllSidebars(); 
            } 
        }

        function toggleCart() { document.getElementById('cart-sidebar').classList.toggle('translate-x-full'); document.getElementById('sidebar-overlay').style.display = document.getElementById('cart-sidebar').classList.contains('translate-x-full') ? 'none' : 'block'; }
        function closeAllSidebars() { document.getElementById('admin-sidebar').classList.add('translate-x-full'); document.getElementById('cart-sidebar').classList.add('translate-x-full'); document.getElementById('sidebar-overlay').style.display = 'none'; }
        function closeModal() { document.getElementById('product-modal').style.display = 'none'; }
        function openCheckout() { if(cart.length === 0) return alert("السلة فارغة!"); document.getElementById('checkout-modal').style.display = 'flex'; closeAllSidebars(); }
        function closeCheckout() { document.getElementById('checkout-modal').style.display = 'none'; }
        function filterBy(c) { activeCat = c; bootApp(); }
        function searchEngine() { const q = document.getElementById('global-search').value.toLowerCase(); renderProducts(products.filter(p => p.name.toLowerCase().includes(q))); }
        function switchAdminTab(tab) { document.getElementById('admin-products-sec').classList.toggle('hidden', tab !== 'products'); document.getElementById('admin-orders-sec').classList.toggle('hidden', tab !== 'orders'); }

        bootApp();
    </script>
</body>
</html>
