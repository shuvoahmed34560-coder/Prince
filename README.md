# Prince
<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gadgets Store</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: #f5f5f5;
        }
        header {
            background-color: #1e90ff;
            color: white;
            padding: 20px;
            text-align: center;
        }
        .products, .cart {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            padding: 20px;
        }
        .product-card, .cart-item {
            background: white;
            margin: 10px;
            padding: 15px;
            border-radius: 10px;
            width: 200px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.2);
            text-align: center;
        }
        .product-card img, .cart-item img {
            width: 100%;
            border-radius: 10px;
        }
        button {
            background-color: #1e90ff;
            color: white;
            border: none;
            padding: 10px;
            margin-top: 10px;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #005bb5;
        }
        h2 {
            text-align: center;
            margin-top: 30px;
        }
    </style>
</head>
<body>
    <header>
        <h1>Gadgets Store</h1>
    </header>

    <h2>প্রোডাক্ট</h2>
    <div class="products">
        <div class="product-card" data-name="ফোন কেস" data-price="500">
            <img src="https://via.placeholder.com/200x150.png?text=ফোন+কেস" alt="Phone Case">
            <h3>ফোন কেস</h3>
            <p>৳500</p>
            <button onclick="addToCart(this)">কার্টে যোগ করুন</button>
        </div>
        <div class="product-card" data-name="হেডফোন" data-price="1200">
            <img src="https://via.placeholder.com/200x150.png?text=হেডফোন" alt="Headphones">
            <h3>হেডফোন</h3>
            <p>৳1200</p>
            <button onclick="addToCart(this)">কার্টে যোগ করুন</button>
        </div>
        <div class="product-card" data-name="স্মার্টওয়াচ" data-price="3500">
            <img src="https://via.placeholder.com/200x150.png?text=স্মার্টওয়াচ" alt="Smartwatch">
            <h3>স্মার্টওয়াচ</h3>
            <p>৳3500</p>
            <button onclick="addToCart(this)">কার্টে যোগ করুন</button>
        </div>
    </div>

    <h2>কার্ট</h2>
    <div class="cart" id="cart"></div>

    <script>
        let cart = [];

        function addToCart(button) {
            const productCard = button.parentElement;
            const name = productCard.getAttribute("data-name");
            const price = parseInt(productCard.getAttribute("data-price"));

            const existing = cart.find(item => item.name === name);
            if(existing){
                existing.qty += 1;
            } else {
                cart.push({name, price, qty: 1});
            }
            renderCart();
        }

        function removeFromCart(index){
            cart.splice(index, 1);
            renderCart();
        }

        function renderCart() {
            const cartDiv = document.getElementById("cart");
            cartDiv.innerHTML = "";

            let total = 0;

            cart.forEach((item, index) => {
                total += item.price * item.qty;

                const div = document.createElement("div");
                div.className = "cart-item";
                div.innerHTML = `
                    <h4>${item.name}</h4>
                    <p>৳${item.price} x ${item.qty}</p>
                    <button onclick="removeFromCart(${index})">মুছে ফেলুন</button>
                `;
                cartDiv.appendChild(div);
            });

            if(cart.length > 0){
                const totalDiv = document.createElement("div");
                totalDiv.style.textAlign = "center";
                totalDiv.style.marginTop = "20px";
                totalDiv.innerHTML = `
                    <h3>মোট: ৳${total}</h3>
                    <button onclick="checkout()">চেকআউট</button>
                `;
                cartDiv.appendChild(totalDiv);
            }
        }

        function checkout(){
            if(cart.length === 0){
                alert("কার্ট খালি!");
                return;
            }

            // PayPal payment integration (sandbox example)
            const total = cart.reduce((sum, item) => sum + item.price * item.qty, 0);
            const paypalUrl = `https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&business=youremail@example.com&item_name=Gadgets+Store+Order&amount=${total}&currency_code=USD`;
            window.open(paypalUrl, "_blank");

            cart = [];
            renderCart();
        }
    </script>
</body>
</html>
