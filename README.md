<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>the local shop</title>
    
    <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        /* Base Styles & Variables */
        :root {
            --primary: #2ecc71;
            --primary-hover: #27ae60;
            --dark: #2c3e50;
            --light: #f8f9fa;
            --white: #ffffff;
            --danger: #e74c3c;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
        }
        
        body {
            background-color: var(--light);
            color: var(--dark);
            overflow-x: hidden;
        }

        /* Hero Section */
        .hero {
            height: 100vh;
            background: linear-gradient(rgba(0,0,0,0.4), rgba(0,0,0,0.7)), url('https://i.ibb.co/SD1PNX7S/Screenshot-20260301-173953.jpg') center/cover no-repeat;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            color: var(--white);
            padding: 0 20px;
        }
        
        .hero h1 {
            font-size: 3.5rem;
            margin-bottom: 10px;
            text-transform: capitalize;
            letter-spacing: 1px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }
        
        .hero p {
            font-size: 1.3rem;
            margin-bottom: 30px;
            font-weight: 300;
        }
        
        .btn {
            padding: 14px 35px;
            background-color: var(--primary);
            color: var(--white);
            border: none;
            border-radius: 30px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            text-decoration: none;
            box-shadow: 0 4px 15px rgba(46, 204, 113, 0.4);
        }
        
        .btn:hover {
            background-color: var(--primary-hover);
            transform: translateY(-2px);
        }

        /* Products Section */
        .products-section {
            padding: 80px 20px;
            max-width: 1200px;
            margin: auto;
        }
        
        .products-section h2 {
            text-align: center;
            margin-bottom: 40px;
            font-size: 2.2rem;
            color: var(--dark);
        }
        
        /* Carousel Container (Horizontal Scroll) */
        .carousel {
            display: flex;
            overflow-x: auto;
            scroll-snap-type: x mandatory;
            gap: 25px;
            padding-bottom: 30px;
            scrollbar-width: thin;
            scrollbar-color: var(--primary) #e0e0e0;
        }
        
        .carousel::-webkit-scrollbar { height: 8px; }
        .carousel::-webkit-scrollbar-track { background: #e0e0e0; border-radius: 4px; }
        .carousel::-webkit-scrollbar-thumb { background: var(--primary); border-radius: 4px; }
        
        .product-card {
            flex: 0 0 85%;
            max-width: 320px;
            scroll-snap-align: center;
            background: var(--white);
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0,0,0,0.08);
            transition: transform 0.3s ease;
        }

        @media (min-width: 768px) {
            .product-card { flex: 0 0 calc(33.333% - 25px); }
        }
        
        .product-img {
            width: 100%;
            height: 280px;
            object-fit: cover;
        }
        
        .product-info {
            padding: 20px;
            text-align: center;
        }
        
        .product-title {
            font-size: 1.25rem;
            margin-bottom: 8px;
            color: var(--dark);
        }
        
        .product-price {
            color: var(--primary);
            font-weight: bold;
            font-size: 1.2rem;
            margin-bottom: 18px;
        }
        
        .add-to-cart-btn {
            width: 100%;
            padding: 12px;
            border-radius: 8px;
        }

        /* Floating Buttons */
        .floating-btns {
            position: fixed;
            bottom: 25px;
            right: 25px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            z-index: 1000;
        }
        
        .float-btn {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: var(--white);
            font-size: 1.8rem;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            text-decoration: none;
            position: relative;
            transition: transform 0.2s ease;
        }
        
        .float-btn:hover {
            transform: scale(1.1);
        }
        
        .whatsapp-btn { background-color: #25D366; }
        .cart-btn { background-color: var(--dark); border: none; }
        
        .cart-badge {
            position: absolute;
            top: -2px;
            right: -2px;
            background-color: var(--danger);
            color: white;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 0.85rem;
            font-weight: bold;
            border: 2px solid var(--light);
        }

        /* Cart Modal / Drawer */
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.6);
            display: none;
            justify-content: flex-end;
            z-index: 1001;
            backdrop-filter: blur(3px);
        }
        
        .cart-drawer {
            width: 100%;
            max-width: 420px;
            height: 100%;
            background: var(--white);
            padding: 25px;
            display: flex;
            flex-direction: column;
            transform: translateX(100%);
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .modal-overlay.active { display: flex; }
        .modal-overlay.active .cart-drawer { transform: translateX(0); }
        
        .cart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px solid var(--light);
            padding-bottom: 15px;
            margin-bottom: 20px;
        }
        
        .close-btn {
            background: none;
            border: none;
            font-size: 2rem;
            color: var(--dark);
            cursor: pointer;
        }
        
        .cart-items {
            flex: 1;
            overflow-y: auto;
            padding-right: 10px;
        }
        
        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            background: var(--light);
            padding: 15px;
            border-radius: 8px;
        }
        
        .cart-item-info { display: flex; flex-direction: column; }
        .cart-item-title { font-weight: 600; margin-bottom: 5px; }
        .cart-item-price { color: var(--primary); font-weight: bold; }
        
        .remove-btn {
            color: var(--danger);
            background: none;
            border: none;
            cursor: pointer;
            font-size: 0.9rem;
            margin-top: 8px;
            text-align: left;
            text-decoration: underline;
        }
        
        .cart-footer {
            border-top: 2px solid var(--light);
            padding-top: 20px;
            margin-top: auto;
        }

        .cart-total {
            font-size: 1.4rem;
            font-weight: bold;
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
        }
        
        /* Order Form */
        .order-form {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        
        .order-form input {
            padding: 14px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
        }
        
        .checkout-btn {
            width: 100%;
            padding: 15px;
            font-size: 1.1rem;
            margin-top: 5px;
        }
    </style>
</head>
<body>

    <section class="hero" id="home">
        <h1>the local shop</h1>
        <p>Fresh Finds, Best Prices</p>
        <button class="btn" onclick="document.getElementById('products').scrollIntoView({behavior: 'smooth'})">Shop Now</button>
    </section>

    <section class="products-section" id="products">
        <h2>Featured Products</h2>
        <div class="carousel" id="product-container">
            </div>
    </section>

    <div class="floating-btns">
        <a href="https://wa.me/99999999" target="_blank" class="float-btn whatsapp-btn" aria-label="Chat on WhatsApp">
            <i class="fab fa-whatsapp"></i>
        </a>
        <button class="float-btn cart-btn" onclick="toggleCart()" aria-label="Open Cart">
            <i class="fas fa-shopping-cart"></i>
            <span class="cart-badge" id="cart-count">0</span>
        </button>
    </div>

    <div class="modal-overlay" id="cart-modal">
        <div class="cart-drawer">
            <div class="cart-header">
                <h2>Your Cart</h2>
                <button class="close-btn" onclick="toggleCart()">&times;</button>
            </div>
            
            <div class="cart-items" id="cart-items-container">
                <p style="text-align: center; color: #7f8c8d; margin-top: 20px;">Your cart is empty.</p>
            </div>
            
            <div class="cart-footer">
                <div class="cart-total">
                    <span>Total:</span>
                    <span>$<span id="cart-total-price">0.00</span></span>
                </div>
                
                <form class="order-form" id="order-form" onsubmit="sendOrder(event)">
                    <input type="text" id="customer-name" placeholder="Your Full Name" required>
                    <input type="email" id="customer-email" placeholder="Your Email Address" required>
                    <button type="submit" class="btn checkout-btn" id="submit-btn">Send Order</button>
                </form>
            </div>
        </div>
    </div>

    <script>
        /* ========================================================
        EMAILJS SETUP INSTRUCTIONS:
        1. Go to emailjs.com and create an account
        2. Uncomment emailjs.init() below and add your PUBLIC KEY
        3. Scroll down to the sendOrder function and add your 
           SERVICE ID and TEMPLATE ID
        ========================================================
        */
        
        // emailjs.init("YOUR_PUBLIC_KEY_HERE");

        // Product Database
        const products = [
            { 
                id: 1, 
                name: "Fresh Catch", 
                price: 15.99, 
                image: "https://i.ibb.co/fYPg031d/17765889123942011803313559717507.jpg" 
            },
            { 
                id: 2, 
                name: "Local Greens", 
                price: 8.50, 
                image: "https://i.ibb.co/fzSztHCT/17765889719475607158956300033953.jpg" 
            },
            { 
                id: 3, 
                name: "Organic Selection", 
                price: 24.00, 
                image: "https://i.ibb.co/MxsZTBLW/17765890123262163223720904162343.jpg" 
            }
        ];

        let cart = [];

        // 1. Render Products into HTML
        const productContainer = document.getElementById('product-container');
        products.forEach(product => {
            productContainer.innerHTML += `
                <div class="product-card">
                    <img src="${product.image}" alt="${product.name}" class="product-img" loading="lazy">
                    <div class="product-info">
                        <h3 class="product-title">${product.name}</h3>
                        <p class="product-price">$${product.price.toFixed(2)}</p>
                        <button class="btn add-to-cart-btn" onclick="addToCart(${product.id}, this)">Add to Cart</button>
                    </div>
                </div>
            `;
        });

        // 2. Add to Cart Logic
        function addToCart(productId, btnElement) {
            const product = products.find(p => p.id === productId);
            const existingItem = cart.find(item => item.id === productId);
            
            if (existingItem) {
                existingItem.quantity += 1;
            } else {
                cart.push({ ...product, quantity: 1 });
            }
            updateCartUI();
            
            // Button visual feedback
            const originalText = btnElement.innerText;
            btnElement.innerText = "Added!";
            btnElement.style.backgroundColor = "var(--dark)";
            
            setTimeout(() => {
                btnElement.innerText = originalText;
                btnElement.style.backgroundColor = "var(--primary)";
            }, 800);
        }

        // 3. Remove from Cart Logic
        function removeFromCart(productId) {
            cart = cart.filter(item => item.id !== productId);
            updateCartUI();
        }

        // 4. Update the Cart User Interface
        function updateCartUI() {
            const cartCount = document.getElementById('cart-count');
            const cartContainer = document.getElementById('cart-items-container');
            const cartTotal = document.getElementById('cart-total-price');

            // Calculate Totals
            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            let totalCost = 0;
            
            cartCount.innerText = totalItems;
            cartContainer.innerHTML = '';

            // Render Cart Items
            if (cart.length === 0) {
                cartContainer.innerHTML = '<p style="text-align: center; color: #7f8c8d; margin-top: 20px;">Your cart is empty.</p>';
            } else {
                cart.forEach(item => {
                    const itemTotal = item.price * item.quantity;
                    totalCost += itemTotal;
                    
                    cartContainer.innerHTML += `
                        <div class="cart-item">
                            <div class="cart-item-info">
                                <span class="cart-item-title">${item.name} <span style="color:#7f8c8d; font-weight:normal">(x${item.quantity})</span></span>
                                <button class="remove-btn" onclick="removeFromCart(${item.id})">Remove</button>
                            </div>
                            <div class="cart-item-price">$${itemTotal.toFixed(2)}</div>
                        </div>
                    `;
                });
            }

            cartTotal.innerText = totalCost.toFixed(2);
        }

        // 5. Toggle Modal Visibility
        function toggleCart() {
            const modal = document.getElementById('cart-modal');
            modal.classList.toggle('active');
        }

        // Close modal if user clicks on the dark overlay background
        document.getElementById('cart-modal').addEventListener('click', function(e) {
            if (e.target === this) toggleCart();
        });

        // 6. Send Order via EmailJS
        function sendOrder(e) {
            e.preventDefault();
            
            if (cart.length === 0) {
                alert("Please add items to your cart before sending an order.");
                return;
            }

            const name = document.getElementById('customer-name').value;
            const email = document.getElementById('customer-email').value;
            const totalCost = document.getElementById('cart-total-price').innerText;
            const submitBtn = document.getElementById('submit-btn');
            
            // Format cart into a clean string for the email body
            let orderDetails = cart.map(item => 
                `- ${item.name} (Quantity: ${item.quantity}) - $${(item.price * item.quantity).toFixed(2)}`
            ).join('\n');

            const templateParams = {
                customer_name: name,
                customer_email: email,
                owner_email: 'thelocalshop@gmail.com', // To email
                order_details: orderDetails,
                total_amount: totalCost
            };

            submitBtn.innerText = "Sending Order...";
            submitBtn.disabled = true;

            /* // =================================================================
            // UNCOMMENT THIS BLOCK WHEN YOU HAVE YOUR EMAILJS ACCOUNT READY
            // =================================================================
            
            emailjs.send('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', templateParams)
                .then(function(response) {
                    alert('Order sent successfully! We will contact you soon.');
                    cart = [];
                    updateCartUI();
                    toggleCart();
                    e.target.reset();
                    submitBtn.innerText = "Send Order";
                    submitBtn.disabled = false;
                }, function(error) {
                    alert('Failed to send order. Please try again.');
                    submitBtn.innerText = "Send Order";
                    submitBtn.disabled = false;
                    console.error('EmailJS Error:', error);
                });
            */

            // Simulation for display purposes (Remove this once EmailJS is hooked up above)
            setTimeout(() => {
                alert(`Test Order Created!\nName: ${name}\nTotal: $${totalCost}\n\nNote: Link your EmailJS keys in the HTML to actually deliver emails.`);
                cart = [];
                updateCartUI();
                toggleCart();
                e.target.reset();
                submitBtn.innerText = "Send Order";
                submitBtn.disabled = false;
            }, 1000);
        }
    </script>
</body>
</html>
