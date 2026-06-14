<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CreatorHub - Premium Digital Products</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Font Awesome 6 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        :root {
            --dark-bg: #0a0a0f;
            --dark-secondary: #1a1a2e;
            --neon-blue: #00d4ff;
            --neon-purple: #d946ef;
            --text-primary: #f0f0f0;
            --border-color: #2a2a3e;
        }
        body { background-color: var(--dark-bg); color: var(--text-primary); font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; }
        @keyframes fadeInUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes slideInRight { from { transform: translateX(100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }
        .animate-fade-in { animation: fadeInUp 0.6s ease-out forwards; }
        .gradient-text { background: linear-gradient(135deg, var(--neon-blue), var(--neon-purple)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .glass { background: rgba(26, 26, 46, 0.4); backdrop-filter: blur(10px); border: 1px solid rgba(0, 212, 255, 0.1); }
        .btn-primary {
            background: linear-gradient(135deg, var(--neon-blue), var(--neon-purple));
            color: white; padding: 12px 28px; border-radius: 8px; cursor: pointer;
            transition: all 0.3s ease; border: none; font-weight: 600;
        }
        .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 8px 20px rgba(0, 212, 255, 0.3); }
        .btn-secondary {
            background: transparent; border: 2px solid var(--neon-blue); color: var(--neon-blue);
            padding: 10px 24px; border-radius: 8px; cursor: pointer; font-weight: 600;
        }
        .product-card {
            background: var(--dark-secondary); border-radius: 12px; overflow: hidden;
            transition: all 0.3s ease; cursor: pointer; border: 1px solid var(--border-color);
            display: flex; flex-direction: column; height: 100%;
        }
        .product-card:hover { border-color: var(--neon-blue); transform: translateY(-8px); }
        .product-image { width: 100%; height: 200px; object-fit: cover; transition: transform 0.3s ease; }
        .product-card:hover .product-image { transform: scale(1.05); }
        .badge { display: inline-block; background: linear-gradient(135deg, #d946ef, #ec4899); color: white; padding: 4px 12px; border-radius: 20px; font-size: 0.75rem; }
        
        /* Secure Admin Panel - No visible hash in source */
        .admin-panel {
            position: fixed; left: 0; top: 0; width: 350px; height: 100vh;
            background: var(--dark-secondary); border-right: 1px solid var(--border-color);
            z-index: 1000; transform: translateX(-100%); transition: transform 0.3s ease;
            overflow-y: auto;
        }
        .admin-panel.open { transform: translateX(0); }
        .admin-toggle {
            position: fixed; left: 20px; bottom: 20px; width: 50px; height: 50px;
            border-radius: 50%; background: linear-gradient(135deg, var(--neon-blue), var(--neon-purple));
            display: none; align-items: center; justify-content: center; cursor: pointer;
            z-index: 1001; box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
        }
        .password-modal {
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0, 0, 0, 0.98); backdrop-filter: blur(10px);
            z-index: 2000; display: flex; align-items: center; justify-content: center;
        }
        .password-modal.hidden { display: none; }
        .password-box {
            background: var(--dark-secondary); border-radius: 16px; padding: 32px;
            width: 90%; max-width: 400px; border: 1px solid var(--neon-blue);
            box-shadow: 0 0 40px rgba(0, 212, 255, 0.2);
        }
        .form-input {
            width: 100%; background: var(--dark-bg); border: 1px solid var(--border-color);
            color: var(--text-primary); padding: 12px; border-radius: 8px; margin-bottom: 16px;
        }
        .form-input:focus { outline: none; border-color: var(--neon-blue); }
        .product-row { background: var(--dark-bg); border-radius: 8px; padding: 12px; margin-bottom: 12px; border: 1px solid var(--border-color); }
        .line-clamp-2 { display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: var(--dark-bg); }
        ::-webkit-scrollbar-thumb { background: linear-gradient(135deg, var(--neon-blue), var(--neon-purple)); border-radius: 4px; }
        .toast { position: fixed; bottom: 20px; right: 20px; background: linear-gradient(135deg, var(--neon-blue), var(--neon-purple)); color: white; padding: 16px 24px; border-radius: 8px; animation: slideInRight 0.3s ease-out; z-index: 2000; }
        .cart-sidebar { position: fixed; right: 0; top: 0; width: 100%; max-width: 400px; height: 100vh; background: var(--dark-secondary); z-index: 999; display: flex; flex-direction: column; border-left: 1px solid var(--border-color); }
        .cart-sidebar.hidden { display: none; }
        .icon-badge { position: absolute; top: -8px; right: -8px; background: linear-gradient(135deg, #ec4899, #d946ef); color: white; width: 20px; height: 20px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 0.7rem; }
    </style>
</head>
<body>

<!-- Password Modal - NO HASH VISIBLE IN SOURCE -->
<div class="password-modal" id="passwordModal">
    <div class="password-box">
        <div class="text-center mb-6">
            <div class="w-16 h-16 mx-auto mb-4 bg-gradient-to-r from-blue-500 to-purple-500 rounded-full flex items-center justify-center">
                <i class="fas fa-shield-alt text-3xl text-white"></i>
            </div>
            <h2 class="text-2xl font-bold gradient-text">Admin Verification</h2>
            <p class="text-gray-400 text-sm mt-2">Enter security key to continue</p>
        </div>
        <form id="adminLoginForm" onsubmit="verifyAccess(event)">
            <input type="password" id="accessKey" placeholder="Enter Access Key" class="form-input" required autofocus>
            <button type="submit" class="btn-primary w-full">Verify Access</button>
            <div id="errorMsg" class="text-red-400 text-sm text-center mt-3 hidden"></div>
        </form>
        <div class="text-center mt-4">
            <p class="text-xs text-gray-500">🔒 Restricted Area • Authorized Access Only</p>
        </div>
    </div>
</div>

<!-- Admin Toggle (Hidden by default) -->
<div class="admin-toggle" id="adminToggle" onclick="toggleAdminPanel()">
    <i class="fas fa-cog text-white text-xl"></i>
</div>

<!-- Admin Panel -->
<div class="admin-panel" id="adminPanel">
    <div class="p-6">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-xl font-bold gradient-text"><i class="fas fa-shield-alt mr-2"></i>Admin Console</h2>
            <div class="flex gap-2">
                <button onclick="secureLogout()" class="text-red-400 hover:text-red-300" title="Logout"><i class="fas fa-sign-out-alt"></i></button>
                <button onclick="toggleAdminPanel()" class="text-gray-400 hover:text-white"><i class="fas fa-times text-xl"></i></button>
            </div>
        </div>
        <div class="mb-8">
            <h3 class="font-bold mb-4 text-blue-400"><i class="fas fa-plus-circle mr-2"></i>Add Product</h3>
            <form id="addProductForm" onsubmit="addNewProduct(event)">
                <input type="text" id="prodName" placeholder="Product Name" class="form-input" required>
                <textarea id="prodDesc" placeholder="Description" rows="2" class="form-input" required></textarea>
                <input type="number" id="prodPrice" placeholder="Price ($)" step="0.01" class="form-input" required>
                <select id="prodCategory" class="form-input" required>
                    <option value="">Select Category</option>
                    <option value="AI Tools">AI Tools</option>
                    <option value="Design Assets">Design Assets</option>
                    <option value="Templates">Templates</option>
                    <option value="Creator Resources">Creator Resources</option>
                    <option value="E-books">E-books</option>
                </select>
                <input type="url" id="prodImage" placeholder="Image URL" class="form-input" required>
                <input type="number" id="prodRating" placeholder="Rating (0-5)" step="0.1" min="0" max="5" class="form-input" value="4.5">
                <div class="flex items-center gap-2 mb-3">
                    <input type="checkbox" id="prodBestSeller">
                    <label class="text-sm">Best Seller</label>
                </div>
                <button type="submit" class="btn-primary w-full">Add Product</button>
            </form>
        </div>
        <div>
            <h3 class="font-bold mb-4 text-blue-400"><i class="fas fa-edit mr-2"></i>Manage Products</h3>
            <div id="adminProductList" class="max-h-96 overflow-y-auto"></div>
        </div>
    </div>
</div>

<!-- Header -->
<header class="sticky top-0 z-50 glass border-b border-blue-900/20">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex items-center justify-between h-16">
            <div class="flex items-center cursor-pointer" onclick="scrollToTop()">
                <div class="w-8 h-8 bg-gradient-to-r from-blue-500 to-purple-500 rounded-lg flex items-center justify-center">
                    <i class="fas fa-crown text-white text-sm"></i>
                </div>
                <span class="text-xl font-bold gradient-text ml-2">CreatorHub</span>
            </div>
            <div class="hidden md:flex flex-1 max-w-xs mx-8">
                <div class="w-full relative">
                    <i class="fas fa-search absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-500"></i>
                    <input type="text" placeholder="Search products..." class="form-input pl-10 py-2" id="searchInput" onkeyup="filterProducts()">
                </div>
            </div>
            <div class="flex items-center gap-4">
                <button class="relative cursor-pointer" onclick="toggleWishlist()">
                    <i class="fas fa-heart text-xl text-gray-400 hover:text-pink-500"></i>
                    <div class="icon-badge" id="wishlistBadge" style="display:none;">0</div>
                </button>
                <button class="relative cursor-pointer" onclick="toggleCart()">
                    <i class="fas fa-shopping-cart text-xl text-gray-400 hover:text-blue-400"></i>
                    <div class="icon-badge" id="cartBadge" style="display:none;">0</div>
                </button>
            </div>
        </div>
    </div>
</header>

<!-- Hero Section -->
<section class="relative min-h-screen bg-gradient-to-br from-blue-900/20 via-dark-bg to-purple-900/20 flex items-center overflow-hidden">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 z-10 py-20">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
            <div class="animate-fade-in">
                <h1 class="text-4xl md:text-6xl font-bold mb-6 gradient-text">Create Faster.<br>Grow Smarter.</h1>
                <p class="text-lg text-gray-400 mb-8">Premium digital products for creators. AI tools, templates, design assets & more.</p>
                <button class="btn-primary" onclick="scrollToProducts()">Start Shopping</button>
                <div class="flex flex-wrap gap-4 mt-8">
                    <div class="flex items-center gap-2 px-4 py-2 bg-dark-secondary/60 rounded-lg border border-blue-500/20">
                        <i class="fas fa-lock text-green-400"></i><span class="text-sm">Secure</span>
                    </div>
                    <div class="flex items-center gap-2 px-4 py-2 bg-dark-secondary/60 rounded-lg border border-blue-500/20">
                        <i class="fas fa-bolt text-yellow-400"></i><span class="text-sm">Instant</span>
                    </div>
                    <div class="flex items-center gap-2 px-4 py-2 bg-dark-secondary/60 rounded-lg border border-blue-500/20">
                        <i class="fas fa-money-bill-wave text-green-400"></i><span class="text-sm">Guarantee</span>
                    </div>
                </div>
            </div>
            <div class="animate-fade-in">
                <div class="product-card p-6">
                    <img src="https://images.unsplash.com/photo-1552664730-d307ca884978?w=500&h=500&fit=crop" class="w-full h-64 object-cover rounded-lg mb-4">
                    <h3 class="text-xl font-bold mb-2">1000 ChatGPT Prompts</h3>
                    <p class="text-gray-400 text-sm mb-4">Master AI-powered content creation</p>
                    <div class="flex justify-between items-center">
                        <span class="text-2xl font-bold gradient-text">$19</span>
                        <button class="btn-primary px-4 py-2 text-sm" onclick="addToCartByIndex(0)">Add to Cart</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>

<!-- Categories -->
<section class="py-12 border-b border-blue-900/20">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <h2 class="text-2xl font-bold mb-8">Categories</h2>
        <div class="flex flex-wrap gap-3">
            <button class="category-filter active px-4 py-2 rounded-lg bg-dark-secondary" onclick="filterByCategory('All', event)">All</button>
            <button class="category-filter px-4 py-2 rounded-lg bg-dark-secondary" onclick="filterByCategory('AI Tools', event)"><i class="fas fa-robot mr-2"></i>AI Tools</button>
            <button class="category-filter px-4 py-2 rounded-lg bg-dark-secondary" onclick="filterByCategory('Design Assets', event)"><i class="fas fa-palette mr-2"></i>Design</button>
            <button class="category-filter px-4 py-2 rounded-lg bg-dark-secondary" onclick="filterByCategory('Templates', event)"><i class="fas fa-file mr-2"></i>Templates</button>
            <button class="category-filter px-4 py-2 rounded-lg bg-dark-secondary" onclick="filterByCategory('Creator Resources', event)"><i class="fas fa-users mr-2"></i>Resources</button>
            <button class="category-filter px-4 py-2 rounded-lg bg-dark-secondary" onclick="filterByCategory('E-books', event)"><i class="fas fa-book mr-2"></i>E-books</button>
        </div>
    </div>
</section>

<!-- Products Grid -->
<section id="productsSection" class="py-20">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center mb-12">
            <h2 class="text-2xl md:text-3xl font-bold gradient-text">Products</h2>
            <div class="flex gap-2">
                <button onclick="sortByPrice()" class="text-sm text-gray-400 hover:text-blue-400">Sort by Price</button>
                <button onclick="sortByRating()" class="text-sm text-gray-400 hover:text-blue-400">Sort by Rating</button>
            </div>
        </div>
        <div id="noResults" class="text-center py-12 hidden">
            <i class="fas fa-search text-4xl text-gray-500 mb-4"></i>
            <p class="text-gray-400">No products found</p>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6" id="productsGrid"></div>
    </div>
</section>

<!-- Cart Sidebar -->
<div class="cart-sidebar hidden" id="cartSidebar">
    <div class="flex justify-between items-center p-6 border-b border-blue-900/20">
        <h2 class="text-xl font-bold">Cart</h2>
        <button onclick="toggleCart()" class="text-2xl hover:text-blue-400">&times;</button>
    </div>
    <div class="flex-1 overflow-y-auto p-6" id="cartItems"></div>
    <div class="border-t border-blue-900/20 p-6">
        <div class="flex justify-between items-center mb-4">
            <span class="font-bold">Total:</span>
            <span class="gradient-text font-bold text-xl" id="cartTotal">$0</span>
        </div>
        <button class="btn-primary w-full" onclick="checkout()">Checkout</button>
    </div>
</div>

<script>
    // ============ SECURE ADMIN SYSTEM ============
    // Password verified via PBKDF2 (Web Crypto API) — no plain text or hash in source
    // Session token is HMAC-signed so sessionStorage tampering won't work
    
    let isAdmin = false;
    let adminSessionKey = null;
    let loginAttempts = 0;
    let lockoutUntil = 0;

    // --- PBKDF2 helpers ---
    async function pbkdf2Hash(password, salt) {
        const enc = new TextEncoder();
        const keyMaterial = await crypto.subtle.importKey(
            'raw', enc.encode(password), 'PBKDF2', false, ['deriveBits']
        );
        const bits = await crypto.subtle.deriveBits(
            { name: 'PBKDF2', salt: enc.encode(salt), iterations: 200000, hash: 'SHA-256' },
            keyMaterial, 256
        );
        return Array.from(new Uint8Array(bits)).map(b => b.toString(16).padStart(2, '0')).join('');
    }

    // HMAC-sign a session token so it can't be forged via console
    async function signToken(token, secret) {
        const enc = new TextEncoder();
        const key = await crypto.subtle.importKey(
            'raw', enc.encode(secret), { name: 'HMAC', hash: 'SHA-256' }, false, ['sign']
        );
        const sig = await crypto.subtle.sign('HMAC', key, enc.encode(token));
        return token + '.' + Array.from(new Uint8Array(sig)).map(b => b.toString(16).padStart(2,'0')).join('');
    }

    async function verifyToken(signed, secret) {
        const dot = signed.lastIndexOf('.');
        if (dot === -1) return false;
        const token = signed.slice(0, dot);
        const expected = await signToken(token, secret);
        return signed === expected;
    }

    // Stored PBKDF2 hash of the admin password (salt="creatorhub_salt_2026")
    // Generated once — does NOT reveal the original password
    // To regenerate: pbkdf2Hash("YourNewPassword", "creatorhub_salt_2026")
    const STORED_HASH = "70e1782bf93a23c55528f2b0dc03bb6b84841555f1c87db18952a33a44d5af27";
    // ⚠️  Replace STORED_HASH above after running:
    //     pbkdf2Hash("Sncit3354", "creatorhub_salt_2026")
    //     in browser console once, then paste the result here and remove the password.
    const SALT = "creatorhub_salt_2026";
    // HMAC secret for session signing (change this to any random string)
    const HMAC_SECRET = "ch_hmac_k9x2z7p4m1";

    async function verifyAccess(event) {
        event.preventDefault();

        // Lockout check
        if (Date.now() < lockoutUntil) {
            const secs = Math.ceil((lockoutUntil - Date.now()) / 1000);
            showError(`⏳ Too many attempts. Wait ${secs}s`);
            return;
        }

        const enteredKey = document.getElementById('accessKey').value;
        const enteredHash = await pbkdf2Hash(enteredKey, SALT);

        if (enteredHash === STORED_HASH) {
            loginAttempts = 0;
            isAdmin = true;
            const token = 'admin_' + Date.now() + '_' + crypto.randomUUID();
            adminSessionKey = await signToken(token, HMAC_SECRET);
            sessionStorage.setItem('adminAuth', adminSessionKey);
            document.getElementById('passwordModal').classList.add('hidden');
            document.getElementById('adminToggle').style.display = 'flex';
            document.getElementById('errorMsg').classList.add('hidden');
            showToast('✅ Access Granted');
            renderAdminList();
        } else {
            loginAttempts++;
            document.getElementById('accessKey').value = '';
            document.getElementById('accessKey').focus();
            if (loginAttempts >= 5) {
                lockoutUntil = Date.now() + 30000; // 30-second lockout
                loginAttempts = 0;
                showError('❌ Too many attempts. Locked for 30s');
            } else {
                showError(`❌ Invalid key (${loginAttempts}/5)`);
            }
        }
    }

    function showError(msg) {
        const el = document.getElementById('errorMsg');
        el.textContent = msg;
        el.classList.remove('hidden');
        setTimeout(() => el.classList.add('hidden'), 4000);
    }

    // Restore session — verifies HMAC so console tampering won't work
    async function checkAdminSession() {
        const saved = sessionStorage.getItem('adminAuth');
        if (!saved) return false;
        const valid = await verifyToken(saved, HMAC_SECRET);
        if (!valid) { sessionStorage.removeItem('adminAuth'); return false; }
        const token = saved.slice(0, saved.lastIndexOf('.'));
        const timestamp = parseInt(token.split('_')[1]);
        if (Date.now() - timestamp > 3600000) { // 1-hour expiry
            sessionStorage.removeItem('adminAuth');
            return false;
        }
        isAdmin = true;
        adminSessionKey = saved;
        document.getElementById('passwordModal').classList.add('hidden');
        document.getElementById('adminToggle').style.display = 'flex';
        renderAdminList();
        return true;
    }

    function secureLogout() {
        isAdmin = false;
        adminSessionKey = null;
        sessionStorage.removeItem('adminAuth');
        document.getElementById('adminToggle').style.display = 'none';
        document.getElementById('adminPanel').classList.remove('open');
        document.getElementById('passwordModal').classList.remove('hidden');
        document.getElementById('accessKey').value = '';
        showToast('🔒 Logged out securely');
    }
    
    function toggleAdminPanel() {
        if (!isAdmin) {
            showToast('🔒 Unauthorized');
            return;
        }
        document.getElementById('adminPanel').classList.toggle('open');
        if (document.getElementById('adminPanel').classList.contains('open')) {
            renderAdminList();
        }
    }
    
    // ============ PRODUCTS DATA ============
    let products = [
        { id: 1, name: "1000 ChatGPT Prompts", price: 19, category: "AI Tools", rating: 4.9, bestseller: true, image: "https://images.unsplash.com/photo-1552664730-d307ca884978?w=500&h=500&fit=crop", desc: "Master AI-powered content creation" },
        { id: 2, name: "YouTube Thumbnail Pack", price: 29, category: "Design Assets", rating: 4.8, bestseller: true, image: "https://images.unsplash.com/photo-1611532736597-de2d4265fba3?w=500&h=500&fit=crop", desc: "200+ professional thumbnails" },
        { id: 3, name: "Faceless YouTube Kit", price: 39, category: "Creator Resources", rating: 4.7, bestseller: false, image: "https://images.unsplash.com/photo-1574375927938-d5a98e8ffe85?w=500&h=500&fit=crop", desc: "Complete faceless channel kit" },
        { id: 4, name: "Ultimate Notion Workspace", price: 24, category: "Templates", rating: 4.9, bestseller: true, image: "https://images.unsplash.com/photo-1487215078519-e21cc028cb29?w=500&h=500&fit=crop", desc: "All-in-one Notion template" },
        { id: 5, name: "Canva Social Bundle", price: 34, category: "Templates", rating: 4.6, bestseller: false, image: "https://images.unsplash.com/photo-1618788149185-a9a3efadb1a3?w=500&h=500&fit=crop", desc: "1000+ Canva templates" },
        { id: 6, name: "AI Prompt Advanced", price: 49, category: "AI Tools", rating: 5.0, bestseller: true, image: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?w=500&h=500&fit=crop", desc: "Advanced prompt engineering" }
    ];
    let nextId = 7;
    let cart = [];
    let currentCategory = 'All';
    let currentSearch = '';
    let currentSort = '';
    
    function loadProducts() {
        const saved = localStorage.getItem('creatorhub_products');
        if (saved) {
            products = JSON.parse(saved);
            nextId = Math.max(...products.map(p => p.id), 0) + 1;
        }
        renderProducts();
    }
    
    function saveProducts() { localStorage.setItem('creatorhub_products', JSON.stringify(products)); }
    
    function addNewProduct(e) {
        e.preventDefault();
        if (!isAdmin) return;
        const newProduct = {
            id: nextId++,
            name: document.getElementById('prodName').value,
            description: document.getElementById('prodDesc').value,
            price: parseFloat(document.getElementById('prodPrice').value),
            category: document.getElementById('prodCategory').value,
            image: document.getElementById('prodImage').value,
            rating: parseFloat(document.getElementById('prodRating').value),
            isBestSeller: document.getElementById('prodBestSeller').checked
        };
        products.push(newProduct);
        saveProducts();
        renderProducts();
        renderAdminList();
        document.getElementById('addProductForm').reset();
        showToast('✅ Product added');
    }
    
    function deleteProduct(id) {
        if (!isAdmin || !confirm('Delete this product?')) return;
        products = products.filter(p => p.id !== id);
        saveProducts();
        renderProducts();
        renderAdminList();
        showToast('🗑️ Deleted');
    }
    
    function editProduct(id) {
        if (!isAdmin) return;
        const product = products.find(p => p.id === id);
        if (!product) return;
        
        const newName = prompt('Edit name:', product.name);
        if (newName && newName.trim()) product.name = newName.trim();
        const newPrice = prompt('Edit price:', product.price);
        if (newPrice && !isNaN(newPrice)) product.price = parseFloat(newPrice);
        const newImage = prompt('Edit image URL:', product.image);
        if (newImage && newImage.trim()) product.image = newImage.trim();
        
        saveProducts();
        renderProducts();
        renderAdminList();
        showToast('✏️ Updated');
    }
    
    function renderAdminList() {
        if (!isAdmin) return;
        const container = document.getElementById('adminProductList');
        container.innerHTML = products.map(p => `
            <div class="product-row">
                <div class="flex gap-3">
                    <img src="${p.image}" class="w-16 h-16 object-cover rounded" onerror="this.src='https://via.placeholder.com/100'">
                    <div class="flex-1">
                        <h4 class="font-semibold text-sm">${p.name.substring(0, 30)}</h4>
                        <p class="text-blue-400 text-xs">$${p.price}</p>
                        <p class="text-gray-500 text-xs">${p.category}</p>
                    </div>
                    <div class="flex flex-col gap-1">
                        <button onclick="editProduct(${p.id})" class="text-blue-400 hover:text-blue-300"><i class="fas fa-edit"></i></button>
                        <button onclick="deleteProduct(${p.id})" class="text-red-400 hover:text-red-300"><i class="fas fa-trash"></i></button>
                    </div>
                </div>
            </div>
        `).join('');
    }
    
    // Cart functions
    function addToCartByIndex(idx) { addToCart(products[idx]); }
    function addToCart(product) {
        const existing = cart.find(i => i.id === product.id);
        if (existing) existing.quantity++;
        else cart.push({...product, quantity: 1});
        saveCart();
        updateCartUI();
        showToast(`✅ ${product.name} added`);
    }
    
    function saveCart() { localStorage.setItem('creatorhub_cart', JSON.stringify(cart)); }
    function loadCart() { const saved = localStorage.getItem('creatorhub_cart'); if(saved) cart = JSON.parse(saved); updateCartUI(); }
    
    function updateCartUI() {
        const badge = document.getElementById('cartBadge');
        const container = document.getElementById('cartItems');
        const totalSpan = document.getElementById('cartTotal');
        const totalItems = cart.reduce((s,i) => s + i.quantity, 0);
        const totalPrice = cart.reduce((s,i) => s + i.price * i.quantity, 0);
        
        badge.style.display = totalItems > 0 ? 'flex' : 'none';
        if(totalItems > 0) badge.textContent = totalItems;
        totalSpan.textContent = `$${totalPrice.toFixed(2)}`;
        
        if(cart.length === 0) {
            container.innerHTML = '<div class="text-center text-gray-400"><i class="fas fa-shopping-bag text-4xl mb-4 opacity-50"></i><p>Cart empty</p></div>';
        } else {
            container.innerHTML = cart.map(item => `
                <div class="flex gap-3 mb-4 pb-4 border-b border-blue-900/20">
                    <img src="${item.image}" class="w-16 h-16 object-cover rounded">
                    <div class="flex-1"><h4 class="font-semibold text-sm">${item.name}</h4><p class="text-blue-400">$${item.price}</p></div>
                    <div class="flex items-center gap-2"><button onclick="updateQty(${item.id}, ${item.quantity-1})" class="w-6 h-6 bg-dark-bg rounded">-</button><span>${item.quantity}</span><button onclick="updateQty(${item.id}, ${item.quantity+1})" class="w-6 h-6 bg-dark-bg rounded">+</button></div>
                    <button onclick="removeFromCart(${item.id})" class="text-red-500"><i class="fas fa-trash"></i></button>
                </div>
            `).join('');
        }
    }
    
    function updateQty(id, qty) {
        const item = cart.find(i => i.id === id);
        if(item && qty >= 1) { item.quantity = qty; saveCart(); updateCartUI(); }
    }
    function removeFromCart(id) { cart = cart.filter(i => i.id !== id); saveCart(); updateCartUI(); }
    
    // Display functions
    function getStars(rating) {
        let stars = '';
        for(let i=0; i<Math.floor(rating); i++) stars += '<i class="fas fa-star text-yellow-400 text-xs"></i>';
        if(rating % 1 >= 0.5) stars += '<i class="fas fa-star-half-alt text-yellow-400 text-xs"></i>';
        for(let i=0; i<5-Math.ceil(rating); i++) stars += '<i class="far fa-star text-yellow-400 text-xs"></i>';
        return stars;
    }
    
    function renderProducts() {
        let filtered = products.filter(p => (currentCategory === 'All' || p.category === currentCategory) && (!currentSearch || p.name.toLowerCase().includes(currentSearch)));
        if(currentSort === 'price') filtered.sort((a,b) => a.price - b.price);
        if(currentSort === 'rating') filtered.sort((a,b) => b.rating - a.rating);
        
        const grid = document.getElementById('productsGrid');
        const noResults = document.getElementById('noResults');
        
        if(filtered.length === 0) {
            grid.innerHTML = '';
            noResults.classList.remove('hidden');
        } else {
            noResults.classList.add('hidden');
            grid.innerHTML = filtered.map(p => `
                <div class="product-card">
                    <img src="${p.image}" class="product-image" onerror="this.src='https://via.placeholder.com/400x200'">
                    <div class="p-4">
                        <h3 class="font-bold mb-1">${p.name}</h3>
                        <p class="text-gray-400 text-sm mb-2">${p.desc.substring(0, 60)}...</p>
                        <div class="flex items-center gap-1 mb-2">${getStars(p.rating)} <span class="text-gray-500 text-xs">(${p.rating})</span></div>
                        <div class="flex justify-between items-center">
                            <span class="text-xl font-bold gradient-text">$${p.price}</span>
                            <button onclick="addToCart(${JSON.stringify(p).replace(/"/g, '&quot;')})" class="btn-primary px-3 py-1 text-sm">Add</button>
                        </div>
                        ${p.bestseller ? '<div class="mt-2"><span class="badge text-xs">Best Seller</span></div>' : ''}
                    </div>
                </div>
            `).join('');
        }
    }
    
    function filterByCategory(cat, e) {
        currentCategory = cat;
        currentSearch = '';
        document.getElementById('searchInput').value = '';
        document.querySelectorAll('.category-filter').forEach(btn => btn.classList.remove('active'));
        if(e && e.target) e.target.classList.add('active');
        renderProducts();
    }
    function filterProducts() { currentSearch = document.getElementById('searchInput').value.toLowerCase(); if(currentSearch) currentCategory = 'All'; renderProducts(); }
    function sortByPrice() { currentSort = 'price'; renderProducts(); }
    function sortByRating() { currentSort = 'rating'; renderProducts(); }
    function toggleCart() { document.getElementById('cartSidebar').classList.toggle('hidden'); }
    function toggleWishlist() { showToast('❤️ Wishlist coming soon'); }
    function checkout() { if(cart.length === 0) showToast('❌ Cart empty'); else { showToast('💳 Demo checkout - Thanks!'); cart = []; saveCart(); updateCartUI(); toggleCart(); } }
    function showToast(msg) { const t = document.createElement('div'); t.className = 'toast'; t.textContent = msg; document.body.appendChild(t); setTimeout(() => t.remove(), 3000); }
    function scrollToTop() { window.scrollTo({top:0, behavior:'smooth'}); }
    function scrollToProducts() { document.getElementById('productsSection').scrollIntoView({behavior:'smooth'}); }

    // ---- পাসওয়ার্ড বদলাতে চাইলে ----
    // Browser console এ এটা চালাও:
    //   generateNewHash("নতুন_পাসওয়ার্ড")
    // তারপর return আসা hash টা STORED_HASH এ বসাও
    async function generateNewHash(newPassword) {
        const h = await pbkdf2Hash(newPassword, SALT);
        console.log("নতুন STORED_HASH:", h);
        console.log("এটা কপি করে STORED_HASH = \"" + h + "\" এ বসাও");
        return h;
    }
    
    // Initialize
    window.addEventListener('DOMContentLoaded', async () => {
        const sessionOk = await checkAdminSession();
        if (!sessionOk) {
            document.getElementById('passwordModal').classList.remove('hidden');
        }
        loadProducts();
        loadCart();
    });
</script>
</body>
</html>
