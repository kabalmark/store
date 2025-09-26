<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mini Online Store</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: "Poppins", sans-serif;
    }
    body {
      background: linear-gradient(135deg, #f5f7fa, #c3cfe2);
      color: #333;
      padding: 20px;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
      font-size: 2.2rem;
      color: #222;
    }
    .store {
      display: flex;
      gap: 20px;
    }
    .products {
      flex: 3;
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 20px;
    }
    .product {
      background: #fff;
      border-radius: 16px;
      overflow: hidden;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      cursor: pointer;
    }
    .product:hover {
      transform: translateY(-8px) scale(1.02);
      box-shadow: 0 8px 20px rgba(0,0,0,0.2);
    }
    .product img {
      width: 100%;
      height: 180px;
      object-fit: cover;
      transition: transform 0.3s ease;
    }
    .product:hover img {
      transform: scale(1.08);
    }
    .product-info {
      padding: 15px;
    }
    .product-info h3 {
      font-size: 1.1rem;
      margin-bottom: 5px;
    }
    .product-info p {
      font-size: 0.9rem;
      margin-bottom: 8px;
      color: #666;
    }
    .product-info button {
      padding: 8px 14px;
      border: none;
      background: linear-gradient(135deg, #667eea, #764ba2);
      color: #fff;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s ease;
    }
    .product-info button:hover {
      background: linear-gradient(135deg, #764ba2, #667eea);
    }
    .cart {
      flex: 1;
      background: #fff;
      border-radius: 16px;
      padding: 20px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      height: fit-content;
    }
    .cart h2 {
      margin-bottom: 15px;
      font-size: 1.4rem;
    }
    .cart ul {
      list-style: none;
      margin-bottom: 15px;
    }
    .cart ul li {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
      font-size: 0.9rem;
    }
    .cart ul li span {
      flex: 1;
    }
    .cart ul li input {
      width: 40px;
      padding: 2px;
      text-align: center;
    }
    .cart-total {
      font-weight: bold;
      margin-bottom: 15px;
      text-align: right;
    }
    .checkout {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    .checkout input, .checkout button {
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 8px;
    }
    .checkout button {
      border: none;
      background: linear-gradient(135deg, #ff758c, #ff7eb3);
      color: #fff;
      font-weight: bold;
      cursor: pointer;
      transition: opacity 0.3s ease;
    }
    .checkout button:hover {
      opacity: 0.9;
    }
  </style>
</head>
<body>
  <h1>Royalty Online Store 🛒</h1>
  <div class="store">
    <!-- Products -->
    <div class="products" id="product-list">
      <div class="product">
        <img src="IMG_1217.jpg" alt="Sneakers">
        <div class="product-info">
          <h3>Stylish Sneakers</h3>
          <p>$50</p>
          <button onclick="addToCart('Stylish Sneakers', 50)">Add to Cart</button>
        </div>
      </div>
      <div class="product">
        <img src="headset.jpg" alt="Headphones">
        <div class="product-info">
          <h3>Wireless Headphones</h3>
          <p>$80</p>
          <button onclick="addToCart('Wireless Headphones', 80)">Add to Cart</button>
        </div>
      </div>
      <div class="product">
        <img src="watch.jpeg">
        <div class="product-info">
          <h3>Luxury Watch</h3>
          <p>$120</p>
          <button onclick="addToCart('Luxury Watch', 120)">Add to Cart</button>
        </div>
      </div>
      <div class="product">
        <img src="leather bag.webp">
        <div class="product-info">
          <h3>Leather Bag</h3>
          <p>$90</p>
          <button onclick="addToCart('Leather Bag', 90)">Add to Cart</button>
        </div>
      </div>
    </div>

    <!-- Cart -->
    <div class="cart">
      <h2>Your Cart</h2>
      <ul id="cart-list"></ul>
      <div class="cart-total">Total: $<span id="total">0</span></div>
      <form class="checkout" onsubmit="handleCheckout(event)">
        <input type="text" id="name" placeholder="Your Name" required>
        <input type="email" id="email" placeholder="Your Email" required>
        <input type="text" id="address" placeholder="Shipping Address" required>
        <button type="submit">Checkout</button>
      </form>
    </div>
  </div>

  <script>
    let cart = [];

    function addToCart(product, price) {
      const item = cart.find(i => i.product === product);
      if (item) {
        item.quantity++;
      } else {
        cart.push({ product, price, quantity: 1 });
      }
      renderCart();
    }

    function updateQuantity(product, quantity) {
      const item = cart.find(i => i.product === product);
      if (item) {
        item.quantity = parseInt(quantity);
        if (item.quantity <= 0) {
          cart = cart.filter(i => i.product !== product);
        }
      }
      renderCart();
    }

    function renderCart() {
      const list = document.getElementById("cart-list");
      list.innerHTML = "";
      let total = 0;
      cart.forEach(item => {
        total += item.price * item.quantity;
        const li = document.createElement("li");
        li.innerHTML = `
          <span>${item.product} - $${item.price}</span>
          <input type="number" value="${item.quantity}" min="1"
            onchange="updateQuantity('${item.product}', this.value)">
        `;
        list.appendChild(li);
      });
      document.getElementById("total").textContent = total;
    }

    function handleCheckout(event) {
      event.preventDefault();
      if (cart.length === 0) {
        alert("Your cart is empty!");
        return;
      }
      const name = document.getElementById("name").value;
      const email = document.getElementById("email").value;
      const address = document.getElementById("address").value;
      alert(`Thank you, ${name}! Your order will be shipped to ${address}.`);
      cart = [];
      renderCart();
      event.target.reset();
    }
  </script>
</body>
</html>
