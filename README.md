<!DOCTYPE html>
<html lang="km">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ហាងអនឡាញអចិន្ត្រៃយ៍ - កំណែកម្រិតខ្ពស់</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Kantumruy+Pro:wght@100;400;700&display=swap');
        body { font-family: 'Kantumruy Pro', sans-serif; }
        .glass-effect { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(10px); }
        .custom-shadow { box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1); }
    </style>
</head>
<body class="bg-slate-50 text-slate-900">

    <div id="app" class="max-w-md mx-auto min-h-screen bg-white shadow-2xl relative pb-24">
        <!-- Header -->
        <nav class="sticky top-0 z-40 glass-effect border-b p-4 flex justify-between items-center">
            <div class="flex items-center gap-3">
                <div class="bg-indigo-600 p-2.5 rounded-2xl shadow-lg shadow-indigo-200">
                    <i class="fas fa-store text-white"></i>
                </div>
                <div>
                    <h1 id="display-shop-name" class="font-bold text-lg leading-tight">កំពុងទាញទិន្នន័យ...</h1>
                    <p class="text-[10px] text-slate-400 uppercase tracking-wider font-bold">Online Store</p>
                </div>
            </div>
            <div class="flex gap-2 text-slate-400">
                <button onclick="openAdmin()" class="p-2.5 hover:text-indigo-600 transition-colors">
                    <i class="fas fa-cog"></i>
                </button>
                <button onclick="toggleCart()" class="relative p-2.5 bg-slate-100 rounded-xl hover:bg-slate-200 transition-colors">
                    <i class="fas fa-shopping-basket text-indigo-600"></i>
                    <span id="cart-count" class="absolute -top-1 -right-1 bg-rose-500 text-white text-[9px] w-5 h-5 rounded-full flex items-center justify-center font-bold border-2 border-white hidden">0</span>
                </button>
            </div>
        </nav>

        <!-- Product Grid -->
        <div id="product-list" class="p-4 grid grid-cols-2 gap-4">
            <div class="col-span-2 flex flex-col items-center justify-center py-20 text-slate-300">
                <i class="fas fa-circle-notch fa-spin text-3xl mb-4"></i>
                <p>កំពុងផ្ទុកទំនិញ...</p>
            </div>
        </div>

        <!-- Cart Drawer -->
        <div id="cart-drawer" class="fixed inset-0 bg-slate-900/60 z-50 hidden flex items-end">
            <div class="bg-white w-full rounded-t-[2.5rem] p-6 max-h-[92vh] overflow-y-auto shadow-2xl">
                <div class="flex justify-between items-center mb-6">
                    <div>
                        <h2 class="font-bold text-xl text-slate-800">កន្ត្រកអីវ៉ាន់</h2>
                        <p class="text-xs text-slate-400">សូមពិនិត្យទំនិញមុននឹងកម្ម៉ង់</p>
                    </div>
                    <button onclick="toggleCart()" class="p-3 bg-slate-100 rounded-full text-slate-500"><i class="fas fa-times"></i></button>
                </div>
                
                <div id="cart-items" class="space-y-4 mb-8"></div>
                
                <div class="space-y-4 border-t pt-6">
                    <div class="grid grid-cols-1 gap-3">
                        <div class="relative">
                            <i class="fas fa-user absolute left-4 top-4 text-slate-300"></i>
                            <input id="cust-name" type="text" placeholder="ឈ្មោះអ្នកទិញ" class="w-full pl-11 pr-4 py-4 bg-slate-50 rounded-2xl border border-slate-100 outline-none focus:border-indigo-400 transition-all">
                        </div>
                        <div class="relative">
                            <i class="fas fa-phone absolute left-4 top-4 text-slate-300"></i>
                            <input id="cust-phone" type="text" placeholder="លេខទូរស័ព្ទ" class="w-full pl-11 pr-4 py-4 bg-slate-50 rounded-2xl border border-slate-100 outline-none focus:border-indigo-400 transition-all">
                        </div>
                        <div class="relative">
                            <i class="fas fa-map-marker-alt absolute left-4 top-4 text-slate-300"></i>
                            <input id="cust-address" type="text" placeholder="អាសយដ្ឋានដឹកជញ្ជូន" class="w-full pl-11 pr-4 py-4 bg-slate-50 rounded-2xl border border-slate-100 outline-none focus:border-indigo-400 transition-all">
                        </div>
                    </div>

                    <div class="p-5 bg-indigo-50 rounded-3xl border border-indigo-100">
                        <div class="flex justify-between items-center mb-4">
                            <span class="text-indigo-900 font-bold">QR បង់ប្រាក់ (ABA/AC)</span>
                            <span class="text-[10px] bg-white px-2 py-1 rounded-full text-indigo-600 font-bold border border-indigo-200 uppercase">Scan to Pay</span>
                        </div>
                        <img id="aba-qr-img" src="" class="mx-auto w-44 h-44 object-contain rounded-2xl shadow-sm bg-white p-2 mb-4" onerror="this.src='https://placehold.co/400x400/4f46e5/white?text=ABA+Pay'">
                        <div class="flex justify-between items-center pt-2 border-t border-indigo-100">
                            <span class="text-slate-500 text-sm">សរុបដែលត្រូវបង់:</span>
                            <span class="font-bold text-2xl text-indigo-700">$<span id="cart-total">0</span></span>
                        </div>
                    </div>
                    
                    <button onclick="submitOrder()" id="btn-order" class="w-full py-5 bg-indigo-600 text-white rounded-[1.5rem] font-bold shadow-lg shadow-indigo-200 hover:bg-indigo-700 active:scale-95 transition-all">
                        <i class="fas fa-paper-plane mr-2"></i> បញ្ជាក់ការកម្ម៉ង់តាម Telegram
                    </button>
                </div>
            </div>
        </div>

        <!-- Admin / Setting Modal -->
        <div id="admin-modal" class="fixed inset-0 bg-slate-900/80 z-[60] hidden flex items-center justify-center p-4">
            <div class="bg-white w-full max-w-sm rounded-[2.5rem] p-6 max-h-[85vh] overflow-y-auto shadow-2xl border border-white/20">
                <!-- Login View -->
                <div id="admin-login" class="text-center py-8">
                    <div class="w-16 h-16 bg-slate-100 rounded-3xl flex items-center justify-center mx-auto mb-6 text-slate-400">
                        <i class="fas fa-lock text-2xl"></i>
                    </div>
                    <h2 class="font-bold text-xl mb-2">ការកំណត់ប្រព័ន្ធ</h2>
                    <p class="text-xs text-slate-400 mb-8">សូមបញ្ចូលលេខកូដសម្ងាត់ដើម្បីកែប្រែ</p>
                    
                    <input id="admin-pass" type="password" class="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl text-center text-xl font-bold mb-6 tracking-widest outline-none focus:border-indigo-500" placeholder="••••">
                    
                    <div class="flex flex-col gap-3">
                        <button onclick="checkPassword()" class="w-full py-4 bg-indigo-600 text-white rounded-2xl font-bold shadow-lg shadow-indigo-100">ចូលទៅកាន់ Setting</button>
                        <button onclick="closeAdmin()" class="py-2 text-slate-400 text-sm font-medium">បោះបង់</button>
                    </div>
                </div>

                <!-- Settings Content -->
                <div id="admin-content" class="hidden space-y-6">
                    <div class="flex justify-between items-center pb-4 border-b">
                        <h2 class="font-bold text-lg text-indigo-900"><i class="fas fa-cog mr-2"></i>Setting ហាង</h2>
                        <button onclick="closeAdmin()" class="text-slate-300 hover:text-slate-600"><i class="fas fa-times"></i></button>
                    </div>

                    <div class="space-y-4">
                        <div>
                            <label class="text-[10px] font-bold text-slate-400 uppercase ml-2 mb-1 block">ឈ្មោះហាង</label>
                            <input id="input-shop-name" type="text" class="w-full p-3.5 bg-slate-50 border border-slate-100 rounded-xl outline-none focus:border-indigo-400 transition-all" placeholder="ឈ្មោះហាង">
                        </div>
                        <div>
                            <label class="text-[10px] font-bold text-slate-400 uppercase ml-2 mb-1 block">Link រូបភាព QR Code (ABA/AC)</label>
                            <input id="input-qr-url" type="text" class="w-full p-3.5 bg-slate-50 border border-slate-100 rounded-xl outline-none focus:border-indigo-400 transition-all" placeholder="https://...">
                        </div>
                    </div>

                    <div class="pt-4">
                        <div class="flex justify-between items-center mb-3">
                            <label class="text-[10px] font-bold text-slate-400 uppercase ml-2">បញ្ជីទំនិញក្នុងហាង</label>
                            <button onclick="addNewProduct()" class="bg-indigo-50 text-indigo-600 px-3 py-1 rounded-lg text-xs font-bold hover:bg-indigo-100 transition-all">
                                <i class="fas fa-plus mr-1"></i> ថែមទំនិញ
                            </button>
                        </div>
                        
                        <div id="admin-product-list" class="space-y-3">
                            <!-- Admin product items will be injected here -->
                        </div>
                    </div>

                    <div class="pt-6 sticky bottom-0 bg-white">
                        <button onclick="saveSettings()" id="btn-save" class="w-full py-4 bg-indigo-600 text-white rounded-2xl font-bold shadow-lg shadow-indigo-200">
                            <i class="fas fa-cloud-upload-alt mr-2"></i> រក្សាទុកក្នុង Cloud
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Success Screen -->
        <div id="success-screen" class="fixed inset-0 bg-white z-[100] hidden flex flex-col items-center justify-center p-10 text-center">
            <div class="w-24 h-24 bg-emerald-100 text-emerald-500 rounded-[2rem] flex items-center justify-center mb-8 shadow-xl shadow-emerald-50 animate-bounce">
                <i class="fas fa-check text-4xl"></i>
            </div>
            <h2 class="text-2xl font-bold text-slate-800 mb-3">កម្ម៉ង់ជោគជ័យ!</h2>
            <p class="text-slate-400 mb-8 leading-relaxed">ពួកយើងបានបញ្ជូនព័ត៌មានទៅកាន់ហាងហើយ។ សូមរង់ចាំការបញ្ជាក់ត្រឡប់មកវិញ។</p>
            <button onclick="location.reload()" class="px-10 py-4 bg-slate-900 text-white rounded-2xl font-bold shadow-xl shadow-slate-200 active:scale-95 transition-all">បន្តទិញអីវ៉ាន់</button>
        </div>
    </div>

    <!-- Firebase SDK Setup -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-app.js";
        import { getFirestore, doc, getDoc, setDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-firestore.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-auth.js";

        // Firebase Configuration with Safe Fallbacks
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
            apiKey: "",
            authDomain: "",
            projectId: "",
            storageBucket: "",
            messagingSenderId: "",
            appId: ""
        };
        
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'global-shop-v2';

        // Local State
        window.shopData = {
            shopName: "My Online Shop",
            qrImageUrl: "https://placehold.co/400x400/indigo/white?text=PAY+HERE",
            products: []
        };
        window.cart = [];

        // Firestore Reference - FOLLOW RULE 1
        const getShopDocRef = () => doc(db, 'artifacts', appId, 'public', 'data', 'config', 'main');

        // Auth Logic - FOLLOW RULE 3
        const initAuth = async () => {
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (error) {
                console.error("Auth initialization failed:", error);
            }
        };

        // Initialize Auth
        initAuth();

        // Listen for Auth State and then start Firestore Operations
        onAuthStateChanged(auth, (user) => {
            if (!user) return; // Wait until authenticated
            
            console.log("Authenticated as:", user.uid);

            // Listen to real-time updates ONLY after auth - FOLLOW RULE 3
            const unsubscribe = onSnapshot(getShopDocRef(), (snapshot) => {
                if (snapshot.exists()) {
                    window.shopData = snapshot.data();
                    renderUI();
                } else {
                    // Seed initial data if document doesn't exist
                    const initialData = {
                        shopName: "ហាងអនឡាញទំនើប",
                        qrImageUrl: "https://placehold.co/400x400/4f46e5/white?text=ABA+Pay",
                        products: [
                            { id: 1, name: "ផលិតផលគំរូ ១", price: 15, image: "https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=400" },
                            { id: 2, name: "ផលិតផលគំរូ ២", price: 25, image: "https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=400" }
                        ]
                    };
                    setDoc(getShopDocRef(), initialData).catch(err => console.error("Initial seed failed:", err));
                }
            }, (error) => {
                // Handle "Missing or insufficient permissions"
                if (error.code === 'permission-denied') {
                    console.error("Firestore Permission Denied. Ensure you are using the correct paths.");
                } else {
                    console.error("Firestore error:", error);
                }
            });

            // Cleanup on page hide/unload if needed
        });

        // UI Functions
        window.renderUI = () => {
            const shopTitle = document.getElementById('display-shop-name');
            if (shopTitle) shopTitle.innerText = window.shopData.shopName;
            
            const container = document.getElementById('product-list');
            if (!container) return;

            if (!window.shopData.products || window.shopData.products.length === 0) {
                container.innerHTML = `<div class="col-span-2 text-center py-20 text-slate-300 italic text-sm">មិនទាន់មានទំនិញលក់នៅឡើយទេ</div>`;
                return;
            }

            container.innerHTML = window.shopData.products.map(p => `
                <div class="bg-white rounded-[2rem] border border-slate-100 overflow-hidden flex flex-col group transition-all hover:shadow-xl hover:shadow-slate-100">
                    <div class="relative overflow-hidden aspect-square">
                        <img src="${p.image}" class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110" onerror="this.src='https://placehold.co/400x400?text=No+Image'">
                        <div class="absolute inset-0 bg-gradient-to-t from-black/20 to-transparent opacity-0 group-hover:opacity-100 transition-opacity"></div>
                    </div>
                    <div class="p-4 flex flex-col flex-grow">
                        <h3 class="font-bold text-xs h-9 line-clamp-2 mb-2 leading-relaxed text-slate-700">${p.name}</h3>
                        <div class="flex justify-between items-center mt-auto pt-2">
                            <span class="font-bold text-indigo-600">$${p.price}</span>
                            <button onclick="addToCart(${p.id})" class="bg-indigo-600 text-white w-9 h-9 rounded-2xl flex items-center justify-center shadow-lg shadow-indigo-100 active:scale-90 transition-all">
                                <i class="fas fa-plus text-xs"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
            
            updateCartUI();
        };

        window.addToCart = (id) => {
            const product = window.shopData.products.find(p => p.id === id);
            if (!product) return;
            const existing = window.cart.find(item => item.id === id);
            if (existing) {
                existing.qty++;
            } else {
                window.cart.push({ ...product, qty: 1 });
            }
            updateCartUI();
        };

        window.updateCartUI = () => {
            const count = window.cart.reduce((sum, item) => sum + item.qty, 0);
            const total = window.cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
            
            const badge = document.getElementById('cart-count');
            if (badge) {
                badge.innerText = count;
                badge.classList.toggle('hidden', count === 0);
            }
            
            const cartTotal = document.getElementById('cart-total');
            if (cartTotal) cartTotal.innerText = total;
            
            const qrImg = document.getElementById('aba-qr-img');
            if (qrImg) qrImg.src = window.shopData.qrImageUrl;
            
            const container = document.getElementById('cart-items');
            if (!container) return;

            if (window.cart.length === 0) {
                container.innerHTML = `<div class="text-center py-10 opacity-30"><i class="fas fa-shopping-cart text-4xl mb-2"></i><p class="text-xs">កន្ត្រកទទេ</p></div>`;
                return;
            }

            container.innerHTML = window.cart.map(item => `
                <div class="flex gap-4 items-center bg-slate-50 p-4 rounded-3xl border border-slate-100">
                    <img src="${item.image}" class="w-14 h-14 rounded-2xl object-cover shadow-sm">
                    <div class="flex-grow min-w-0">
                        <p class="text-xs font-bold text-slate-800 truncate mb-1">${item.name}</p>
                        <p class="text-indigo-600 font-bold">$${item.price * item.qty}</p>
                    </div>
                    <div class="flex items-center gap-3 bg-white px-2 py-1.5 rounded-xl border border-slate-200">
                        <button onclick="changeQty(${item.id}, -1)" class="w-6 h-6 flex items-center justify-center text-slate-400 hover:text-indigo-600 transition-colors"><i class="fas fa-minus text-[10px]"></i></button>
                        <span class="text-xs font-bold w-4 text-center">${item.qty}</span>
                        <button onclick="changeQty(${item.id}, 1)" class="w-6 h-6 flex items-center justify-center text-slate-400 hover:text-indigo-600 transition-colors"><i class="fas fa-plus text-[10px]"></i></button>
                    </div>
                </div>
            `).join('');
        };

        window.changeQty = (id, delta) => {
            const item = window.cart.find(i => i.id === id);
            if (!item) return;
            item.qty += delta;
            if (item.qty <= 0) window.cart = window.cart.filter(i => i.id !== id);
            updateCartUI();
        };

        window.toggleCart = () => document.getElementById('cart-drawer').classList.toggle('hidden');
        window.openAdmin = () => {
            document.getElementById('admin-modal').classList.remove('hidden');
            document.getElementById('admin-login').classList.remove('hidden');
            document.getElementById('admin-content').classList.add('hidden');
            document.getElementById('admin-pass').value = '';
        };
        window.closeAdmin = () => document.getElementById('admin-modal').classList.add('hidden');

        window.checkPassword = () => {
            const pass = document.getElementById('admin-pass').value;
            if (pass === "1234") {
                document.getElementById('admin-login').classList.add('hidden');
                document.getElementById('admin-content').classList.remove('hidden');
                document.getElementById('input-shop-name').value = window.shopData.shopName;
                document.getElementById('input-qr-url').value = window.shopData.qrImageUrl;
                renderAdminProducts();
            } else {
                alert("លេខកូដសម្ងាត់មិនត្រឹមត្រូវ!");
            }
        };

        window.renderAdminProducts = () => {
            const list = document.getElementById('admin-product-list');
            list.innerHTML = (window.shopData.products || []).map((p, index) => `
                <div class="p-4 bg-slate-50 rounded-2xl border border-slate-100 space-y-3 relative group">
                    <button onclick="removeProduct(${index})" class="absolute -top-2 -right-2 w-7 h-7 bg-white border border-rose-100 text-rose-500 rounded-full shadow-sm flex items-center justify-center hover:bg-rose-50 transition-all opacity-0 group-hover:opacity-100">
                        <i class="fas fa-times text-[10px]"></i>
                    </button>
                    <div class="grid grid-cols-3 gap-2">
                        <div class="col-span-2">
                            <label class="text-[9px] font-bold text-slate-400 block ml-1 uppercase">ឈ្មោះទំនិញ</label>
                            <input oninput="updateTempProduct(${index}, 'name', this.value)" value="${p.name}" class="w-full p-2 bg-white border rounded-lg text-xs outline-none focus:border-indigo-400">
                        </div>
                        <div>
                            <label class="text-[9px] font-bold text-slate-400 block ml-1 uppercase">តម្លៃ ($)</label>
                            <input oninput="updateTempProduct(${index}, 'price', this.value)" value="${p.price}" type="number" class="w-full p-2 bg-white border rounded-lg text-xs outline-none focus:border-indigo-400">
                        </div>
                    </div>
                    <div>
                        <label class="text-[9px] font-bold text-slate-400 block ml-1 uppercase">Link រូបភាព (URL)</label>
                        <input oninput="updateTempProduct(${index}, 'image', this.value)" value="${p.image}" class="w-full p-2 bg-white border rounded-lg text-[10px] outline-none focus:border-indigo-400" placeholder="https://...">
                    </div>
                </div>
            `).join('');
        };

        window.updateTempProduct = (index, field, value) => {
            if (!window.shopData.products[index]) return;
            window.shopData.products[index][field] = field === 'price' ? Number(value) : value;
        };

        window.removeProduct = (index) => {
            window.shopData.products.splice(index, 1);
            renderAdminProducts();
        };

        window.addNewProduct = () => {
            if (!window.shopData.products) window.shopData.products = [];
            window.shopData.products.unshift({
                id: Date.now(),
                name: "ទំនិញថ្មី",
                price: 0,
                image: "https://placehold.co/400x400?text=New+Item"
            });
            renderAdminProducts();
        };

        window.saveSettings = async () => {
            if (!auth.currentUser) {
                alert("សូមរង់ចាំការភ្ជាប់ទៅកាន់ប្រព័ន្ធ...");
                return;
            }
            
            const btn = document.getElementById('btn-save');
            btn.disabled = true;
            btn.innerHTML = `<i class="fas fa-circle-notch fa-spin mr-2"></i> កំពុងរក្សាទុក...`;

            const finalData = {
                shopName: document.getElementById('input-shop-name').value || "Online Shop",
                qrImageUrl: document.getElementById('input-qr-url').value || "https://placehold.co/400x400/indigo/white?text=PAY+HERE",
                products: window.shopData.products
            };

            try {
                await setDoc(getShopDocRef(), finalData);
                closeAdmin();
            } catch (err) {
                console.error("Save Error:", err);
                alert("រក្សាទុកបរាជ័យ: " + err.message);
            } finally {
                btn.disabled = false;
                btn.innerHTML = `<i class="fas fa-cloud-upload-alt mr-2"></i> រក្សាទុកក្នុង Cloud`;
            }
        };

        window.submitOrder = async () => {
            const name = document.getElementById('cust-name').value;
            const phone = document.getElementById('cust-phone').value;
            const address = document.getElementById('cust-address').value || "ផ្ញើតាមដឹកជញ្ជូន";
            
            if (!name || !phone || window.cart.length === 0) {
                alert("សូមបំពេញឈ្មោះ លេខទូរស័ព្ទ និងជ្រើសរើសទំនិញ!");
                return;
            }

            const btn = document.getElementById('btn-order');
            btn.disabled = true;
            btn.innerHTML = `<i class="fas fa-circle-notch fa-spin mr-2"></i> កំពុងផ្ញើ...`;

            const TELEGRAM_BOT_TOKEN = "8313263576:AAEqP3T5ycuQP0bi6k0NTmJYbeVyBJCGBbY";
            const TELEGRAM_CHAT_ID = "1859589466";
            
            let itemsText = window.cart.map(i => `• ${i.name} (${i.qty}x) = $${i.price * i.qty}`).join('\n');
            const total = document.getElementById('cart-total').innerText;
            
            const text = `🛍️ **ការកម្ម៉ង់ថ្មីពី ${window.shopData.shopName}**\n\n👤 ឈ្មោះ: ${name}\n📞 ទូរស័ព្ទ: ${phone}\n📍 អាសយដ្ឋាន: ${address}\n\n📦 **ទំនិញ:**\n${itemsText}\n\n💰 **សរុប: $${total}**\n\n⏰ ម៉ោង: ${new Date().toLocaleString('km-KH')}`;

            try {
                const res = await fetch(`https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        chat_id: TELEGRAM_CHAT_ID,
                        text: text,
                        parse_mode: 'Markdown'
                    })
                });

                if (res.ok) {
                    document.getElementById('success-screen').classList.remove('hidden');
                    window.cart = [];
                    updateCartUI();
                } else {
                    throw new Error("Failed to send to Telegram");
                }
            } catch (e) {
                alert("មានបញ្ហាក្នុងការផ្ញើ! សូមព្យាយាមម្តងទៀត។");
            } finally {
                btn.disabled = false;
                btn.innerHTML = `<i class="fas fa-paper-plane mr-2"></i> បញ្ជាក់ការកម្ម៉ង់តាម Telegram`;
            }
        };
    </script>
</body>
</html>
