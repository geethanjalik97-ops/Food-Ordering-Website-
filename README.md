<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Online Food Ordering</title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>

  <!-- Header / Navigation -->
  <header class="header">
    <div class="logo">
      <h1>Tasty<span>Bites</span></h1>
    </div>
    <div class="cart-btn-container">
      <button id="cart-toggle-btn" class="cart-btn">
        🛒 Cart (<span id="cart-count">0</span>)
      </button>
    </div>
  </header>

  <!-- Main Container -->
  <main class="container">
    <!-- Controls Section: Search & Filter -->
    <section class="controls-section">
      <div class="search-box">
        <input 
          type="text" 
          id="search-input" 
          placeholder="Search food items by name..." 
          aria-label="Search menu items"
          aria-controls="search-results"
          autocomplete="off"
        />
        <div id="search-results" class="search-results" aria-label="Matching menu items" aria-live="polite"></div>
      </div>
      <div class="category-filters" id="category-filters">
        <button class="filter-btn active" data-category="All">All</button>
        <button class="filter-btn" data-category="Burgers">Burgers</button>
        <button class="filter-btn" data-category="Pizza">Pizza</button>
        <button class="filter-btn" data-category="Drinks">Drinks</button>
        <button class="filter-btn" data-category="Desserts">Desserts</button>
      </div>
    </section>

    <!-- Food Menu Grid -->
    <section class="food-section">
      <h2>Menu Items</h2>
      <div id="food-grid" class="food-grid">
        <!-- Dynamically populated via JS -->
      </div>
    </section>

    <!-- Shopping Cart Modal / Sidebar -->
    <aside id="cart-sidebar" class="cart-sidebar">
      <div class="cart-header">
        <h2>Your Shopping Cart</h2>
        <button id="close-cart-btn" class="close-btn">&times;</button>
      </div>

      <div id="cart-items" class="cart-items">
        <!-- Dynamically populated via JS -->
      </div>

      <div class="cart-footer">
        <div class="cart-summary">
          <p><span>Subtotal:</span> ₹<span id="cart-subtotal">0.00</span></p>
          <p><span>Taxes (5%):</span> ₹<span id="cart-tax">0.00</span></p>
          <h3><span>Grand Total:</span> ₹<span id="cart-total">0.00</span></h3>
        </div>

        <!-- Checkout / Customer Details Form -->
        <form id="checkout-form" class="checkout-form">
          <h3>Customer Details</h3>
          
          <div class="form-group">
            <label for="cust-name">Full Name *</label>
            <input type="text" id="cust-name" placeholder="Enter your full name" required />
            <span class="error-msg" id="name-error"></span>
          </div>

          <div class="form-group">
            <label for="cust-phone">Phone Number *</label>
            <input type="tel" id="cust-phone" placeholder="10-digit phone number" required />
            <span class="error-msg" id="phone-error"></span>
          </div>

          <div class="form-group">
            <label for="cust-address">Delivery Address *</label>
            <textarea id="cust-address" rows="2" placeholder="Enter full street address" required></textarea>
            <span class="error-msg" id="address-error"></span>
          </div>

          <button type="submit" id="place-order-btn" class="btn-primary">Place Order</button>
        </form>
      </div>
    </aside>

    <div id="overlay" class="overlay"></div>

    <!-- Order Summary Modal (Shown after successful order) -->
    <div id="order-modal" class="modal">
      <div class="modal-content">
        <h2>🎉 Order Placed Successfully!</h2>
        <p id="order-id-display"></p>
        <div id="order-summary-details"></div>
        <button id="close-modal-btn" class="btn-primary">Close & Order Again</button>
      </div>
    </div>
  </main>

  <script src="script.js"></script>
</body>
</html>
/* Fresh red-and-green food ordering theme */
:root {
  --red: #d93636;
  --red-dark: #b9272e;
  --green: #20834b;
  --green-dark: #17683b;
  --green-light: #e8f5eb;
  --ink: #26382d;
  --muted: #68766d;
  --surface: #fffefa;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
  min-height: 100vh;
  color: var(--ink);
  background: linear-gradient(135deg, #fff7f2 0%, #f0faef 100%);
}

button, input, textarea { font: inherit; }
button { cursor: pointer; }

.header {
  position: sticky;
  top: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px max(24px, calc((100% - 1200px) / 2));
  background: var(--surface);
  border-bottom: 4px solid var(--green);
  box-shadow: 0 4px 14px rgba(38, 56, 45, .1);
}

.logo h1 { color: var(--green); font-size: 1.8rem; letter-spacing: -.5px; }
.logo span { color: var(--red); }
.cart-btn {
  padding: 11px 19px;
  color: white;
  background: var(--red);
  border: 0;
  border-radius: 25px;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(217, 54, 54, .2);
  transition: background .2s, transform .2s;
}
.cart-btn:hover { background: var(--red-dark); transform: translateY(-1px); }

.container { max-width: 1200px; margin: 30px auto; padding: 0 20px; }
.controls-section { display: flex; flex-direction: column; gap: 16px; margin-bottom: 32px; }
.search-box { position: relative; }
.search-box input {
  width: 100%;
  padding: 14px 18px;
  color: var(--ink);
  background: white;
  border: 2px solid #d9e7dc;
  border-radius: 12px;
  outline: none;
}
.search-box input:focus { border-color: var(--green); box-shadow: 0 0 0 3px rgba(32, 131, 75, .12); }
.search-results {
  position: absolute;
  top: calc(100% + 4px);
  right: 0;
  left: 0;
  z-index: 20;
  display: none;
  max-height: 260px;
  overflow-y: auto;
  background: white;
  border: 1px solid #c9dfcd;
  border-radius: 10px;
  box-shadow: 0 8px 20px rgba(35, 73, 47, .14);
}
.search-results.show { display: block; }
.search-result {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 12px 16px;
  color: var(--ink);
  background: white;
  border: 0;
  text-align: left;
}
.search-result + .search-result { border-top: 1px solid #e8eee7; }
.search-result:hover, .search-result:focus { background: var(--green-light); outline: none; }
.search-result strong { color: var(--red-dark); white-space: nowrap; }
.search-empty { padding: 12px 16px; color: var(--muted); }
.category-filters { display: flex; gap: 10px; flex-wrap: wrap; }
.filter-btn {
  padding: 8px 17px;
  color: var(--green-dark);
  background: white;
  border: 1px solid #c9dfcd;
  border-radius: 20px;
  font-weight: 600;
  transition: .2s;
}
.filter-btn:hover, .filter-btn.active { color: white; background: var(--green); border-color: var(--green); }

.food-section h2 { margin-bottom: 16px; color: var(--red-dark); font-size: 1.7rem; }
.food-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 20px; }
.food-card {
  overflow: hidden;
  display: flex;
  flex-direction: column;
  background: white;
  border: 1px solid #e8eee7;
  border-radius: 15px;
  box-shadow: 0 5px 16px rgba(35, 73, 47, .09);
  transition: transform .2s, box-shadow .2s;
}
.food-card:hover { transform: translateY(-4px); box-shadow: 0 10px 22px rgba(35, 73, 47, .15); }
.food-card img { width: 100%; height: 180px; object-fit: cover; background: var(--green-light); }
.food-info { display: flex; flex: 1; flex-direction: column; padding: 16px; }
.food-badge { align-self: flex-start; margin-bottom: 7px; padding: 4px 9px; color: var(--green-dark); background: var(--green-light); border-radius: 20px; font-size: .75rem; font-weight: 700; }
.food-title { margin-bottom: 6px; font-size: 1.1rem; }
.food-desc { flex: 1; margin-bottom: 16px; color: var(--muted); font-size: .9rem; line-height: 1.45; }
.food-bottom { display: flex; align-items: center; justify-content: space-between; gap: 8px; }
.food-price { color: var(--red-dark); font-size: 1.1rem; font-weight: 800; }
.add-btn, .btn-primary {
  padding: 9px 13px;
  color: white;
  background: var(--green);
  border: 0;
  border-radius: 8px;
  font-weight: 700;
  transition: background .2s;
}
.add-btn:hover, .btn-primary:hover { background: var(--green-dark); }
.empty-message { grid-column: 1 / -1; padding: 24px; color: var(--muted); text-align: center; }

.cart-sidebar {
  position: fixed;
  top: 0;
  right: -440px;
  z-index: 200;
  display: flex;
  flex-direction: column;
  width: 420px;
  max-width: 100%;
  height: 100%;
  background: var(--surface);
  box-shadow: -5px 0 22px rgba(0, 0, 0, .2);
  transition: right .3s ease-in-out;
}
.cart-sidebar.open { right: 0; }
.cart-header { display: flex; align-items: center; justify-content: space-between; padding: 16px 20px; color: white; background: var(--green); }
.cart-header h2 { font-size: 1.2rem; }
.close-btn { color: white; background: transparent; border: 0; font-size: 1.8rem; }
.cart-items { flex: 1; overflow-y: auto; padding: 16px 20px; }
.cart-item { display: flex; align-items: center; justify-content: space-between; gap: 10px; margin-bottom: 14px; padding-bottom: 13px; border-bottom: 1px solid #e5eee6; }
.cart-item-details h4 { margin-bottom: 4px; font-size: .95rem; }
.cart-item-details p { color: var(--muted); font-size: .85rem; }
.quantity-controls { display: flex; align-items: center; gap: 8px; margin-top: 8px; }
.qty-btn { width: 25px; height: 25px; color: var(--green-dark); background: var(--green-light); border: 0; border-radius: 50%; font-weight: 800; }
.remove-btn { color: var(--red); background: none; border: 0; font-size: .8rem; }
.cart-footer { padding: 14px 20px; border-top: 1px solid #dce8dd; background: #f8fcf7; }
.cart-summary p, .cart-summary h3 { display: flex; justify-content: space-between; margin-bottom: 5px; }
.cart-summary h3 { margin-top: 10px; padding-top: 8px; border-top: 1px solid #dce8dd; }
.checkout-form { max-height: 43vh; overflow-y: auto; margin-top: 14px; }
.checkout-form h3 { margin-bottom: 10px; color: var(--green-dark); font-size: 1rem; }
.form-group { margin-bottom: 9px; }
.form-group label { display: block; margin-bottom: 4px; font-size: .85rem; font-weight: 600; }
.form-group input, .form-group textarea { width: 100%; padding: 8px 10px; border: 1px solid #cddbcf; border-radius: 6px; resize: vertical; }
.form-group input:focus, .form-group textarea:focus { outline: 2px solid #b8dfc1; border-color: var(--green); }
.error-msg { display: block; margin-top: 3px; color: var(--red); font-size: .8rem; }
.checkout-form .btn-primary { width: 100%; margin-top: 4px; }
.overlay { position: fixed; inset: 0; z-index: 150; display: none; background: rgba(21, 37, 26, .45); }
.overlay.show { display: block; }
.modal { position: fixed; inset: 0; z-index: 300; display: none; align-items: center; justify-content: center; padding: 20px; background: rgba(21, 37, 26, .55); }
.modal.show { display: flex; }
.modal-content { width: min(480px, 100%); max-height: 85vh; overflow-y: auto; padding: 28px; background: white; border-top: 6px solid var(--green); border-radius: 14px; box-shadow: 0 12px 35px rgba(0, 0, 0, .2); }
.modal-content h2 { margin-bottom: 12px; color: var(--green-dark); }
.modal-content p { margin: 8px 0; }
.modal-content .btn-primary { margin-top: 14px; }

@media (max-width: 600px) {
  .header { padding: 13px 16px; }
  .logo h1 { font-size: 1.45rem; }
  .cart-btn { padding: 9px 13px; font-size: .9rem; }
  .container { margin-top: 22px; padding: 0 14px; }
  .food-grid { grid-template-columns: 1fr; }
  .cart-sidebar { width: 100%; }
}
// --- Menu data ---
const foodItems = [
  { id: 1, name: "Classic Cheeseburger", category: "Burgers", price: 180, description: "Juicy beef patty with cheddar, lettuce, tomato, and signature sauce.", image: "https://images.unsplash.com/photo-1568901346375-23c9450c58cd?w=400" },
  { id: 2, name: "Margherita Pizza", category: "Pizza", price: 290, description: "Fresh mozzarella, ripe tomatoes, and fragrant basil.", image: "https://images.unsplash.com/photo-1604382354936-07c5d9983bd3?w=400" },
  { id: 3, name: "Pepperoni Passion", category: "Pizza", price: 350, description: "Spicy pepperoni, rich tomato sauce, and double mozzarella.", image: "https://images.unsplash.com/photo-1628840042765-356cda07504e?w=400" },
  { id: 4, name: "Crispy Chicken Burger", category: "Burgers", price: 210, description: "Crispy chicken fillet with mayo, pickles, and lettuce.", image: "https://images.unsplash.com/photo-1521305916504-4a1121188589?w=400" },
  { id: 5, name: "Iced Cold Coffee", category: "Drinks", price: 120, description: "Espresso blended with chilled milk and chocolate drizzle.", image: "https://images.unsplash.com/photo-1517701604599-bb29b565090c?w=400" },
  { id: 6, name: "Fresh Orange Juice", category: "Drinks", price: 90, description: "Freshly squeezed orange juice with no added sugar.", image: "https://images.unsplash.com/photo-1613478223719-2ab802602423?w=400" },
  { id: 7, name: "Chocolate Lava Cake", category: "Desserts", price: 150, description: "Warm chocolate cake with a gooey dark chocolate center.", image: "https://images.unsplash.com/photo-1606313564200-e75d5e30476c?w=400" },
  { id: 8, name: "Vanilla Ice Cream Sundae", category: "Desserts", price: 130, description: "Creamy vanilla with fudge sauce and crunchy nuts.", image: "https://images.unsplash.com/photo-1563805042-7684c019e1cb?w=400" }
];

let cart = [];
let currentCategory = "All";
let searchQuery = "";

const foodGrid = document.getElementById("food-grid");
const searchInput = document.getElementById("search-input");
const searchResults = document.getElementById("search-results");
const categoryFilters = document.getElementById("category-filters");
const cartSidebar = document.getElementById("cart-sidebar");
const overlay = document.getElementById("overlay");
const cartToggleBtn = document.getElementById("cart-toggle-btn");
const closeCartBtn = document.getElementById("close-cart-btn");
const cartItemsElement = document.getElementById("cart-items");
const checkoutForm = document.getElementById("checkout-form");
const orderModal = document.getElementById("order-modal");

const formatPrice = (amount) => amount.toFixed(2);

function renderSearchResults() {
  if (!searchQuery) {
    searchResults.innerHTML = "";
    searchResults.classList.remove("show");
    return;
  }

  const matches = foodItems.filter((item) => item.name.toLowerCase().includes(searchQuery));
  searchResults.innerHTML = matches.length
    ? matches.map((item) => `
      <button class="search-result" type="button" data-search-item="${item.id}">
        <span>${item.name}</span><strong>₹${formatPrice(item.price)}</strong>
      </button>
    `).join("")
    : '<p class="search-empty">No matching menu items.</p>';
  searchResults.classList.add("show");
}

function renderMenu() {
  const filteredItems = foodItems.filter((item) => {
    const matchesCategory = currentCategory === "All" || item.category === currentCategory;
    const matchesSearch = item.name.toLowerCase().includes(searchQuery);
    return matchesCategory && matchesSearch;
  });

  foodGrid.innerHTML = filteredItems.length
    ? filteredItems.map((item) => `
      <article class="food-card" data-food-id="${item.id}">
        <img src="${item.image}" alt="${item.name}" loading="lazy" />
        <div class="food-info">
          <span class="food-badge">${item.category}</span>
          <h3 class="food-title">${item.name}</h3>
          <p class="food-desc">${item.description}</p>
          <div class="food-bottom">
            <span class="food-price">₹${formatPrice(item.price)}</span>
            <button class="add-btn" type="button" data-add="${item.id}">Add to Cart</button>
          </div>
        </div>
      </article>
    `).join("")
    : '<p class="empty-message">No menu items found. Try another search.</p>';
}

function renderCart() {
  const itemCount = cart.reduce((total, item) => total + item.quantity, 0);
  const subtotal = cart.reduce((total, item) => total + item.price * item.quantity, 0);
  const tax = subtotal * 0.05;

  document.getElementById("cart-count").textContent = itemCount;
  document.getElementById("cart-subtotal").textContent = formatPrice(subtotal);
  document.getElementById("cart-tax").textContent = formatPrice(tax);
  document.getElementById("cart-total").textContent = formatPrice(subtotal + tax);

  cartItemsElement.innerHTML = cart.length
    ? cart.map((item) => `
      <div class="cart-item">
        <div class="cart-item-details">
          <h4>${item.name}</h4>
          <p>₹${formatPrice(item.price)} each</p>
          <div class="quantity-controls">
            <button class="qty-btn" type="button" data-quantity="-1" data-id="${item.id}" aria-label="Decrease ${item.name} quantity">−</button>
            <span>${item.quantity}</span>
            <button class="qty-btn" type="button" data-quantity="1" data-id="${item.id}" aria-label="Increase ${item.name} quantity">+</button>
            <button class="remove-btn" type="button" data-remove="${item.id}">Remove</button>
          </div>
        </div>
        <strong>₹${formatPrice(item.price * item.quantity)}</strong>
      </div>
    `).join("")
    : '<p class="empty-message">Your cart is empty. Add something tasty!</p>';
}

function openCart() {
  cartSidebar.classList.add("open");
  overlay.classList.add("show");
}

function closeCart() {
  cartSidebar.classList.remove("open");
  overlay.classList.remove("show");
}

foodGrid.addEventListener("click", (event) => {
  const button = event.target.closest("[data-add]");
  if (!button) return;

  const item = foodItems.find((food) => food.id === Number(button.dataset.add));
  const cartItem = cart.find((food) => food.id === item.id);
  if (cartItem) cartItem.quantity += 1;
  else cart.push({ ...item, quantity: 1 });
  renderCart();
  button.textContent = "Added!";
  window.setTimeout(() => { button.textContent = "Add to Cart"; }, 900);
});

cartItemsElement.addEventListener("click", (event) => {
  const quantityButton = event.target.closest("[data-quantity]");
  const removeButton = event.target.closest("[data-remove]");

  if (quantityButton) {
    const item = cart.find((food) => food.id === Number(quantityButton.dataset.id));
    item.quantity += Number(quantityButton.dataset.quantity);
    if (item.quantity <= 0) cart = cart.filter((food) => food.id !== item.id);
  }
  if (removeButton) cart = cart.filter((food) => food.id !== Number(removeButton.dataset.remove));
  renderCart();
});

searchInput.addEventListener("input", () => {
  searchQuery = searchInput.value.trim().toLowerCase();
  renderSearchResults();
  renderMenu();
});

searchResults.addEventListener("click", (event) => {
  const button = event.target.closest("[data-search-item]");
  if (!button) return;

  const item = foodItems.find((food) => food.id === Number(button.dataset.searchItem));
  searchInput.value = item.name;
  searchQuery = item.name.toLowerCase();
  currentCategory = "All";
  categoryFilters.querySelectorAll(".filter-btn").forEach((filter) => {
    filter.classList.toggle("active", filter.dataset.category === "All");
  });
  searchResults.innerHTML = "";
  searchResults.classList.remove("show");
  renderMenu();
  foodGrid.querySelector(`[data-food-id="${item.id}"]`)?.scrollIntoView({ behavior: "smooth", block: "center" });
});

categoryFilters.addEventListener("click", (event) => {
  const button = event.target.closest("[data-category]");
  if (!button) return;
  currentCategory = button.dataset.category;
  categoryFilters.querySelectorAll(".filter-btn").forEach((filter) => {
    filter.classList.toggle("active", filter === button);
  });
  renderMenu();
});

cartToggleBtn.addEventListener("click", openCart);
closeCartBtn.addEventListener("click", closeCart);
overlay.addEventListener("click", closeCart);

document.getElementById("close-modal-btn").addEventListener("click", () => {
  orderModal.classList.remove("show");
});

checkoutForm.addEventListener("submit", (event) => {
  event.preventDefault();
  if (!cart.length) {
    openCart();
    cartItemsElement.insertAdjacentHTML("afterbegin", '<p class="error-msg">Please add at least one item before placing your order.</p>');
    return;
  }

  const phone = document.getElementById("cust-phone").value.trim();
  if (!/^\d{10}$/.test(phone)) {
    document.getElementById("phone-error").textContent = "Enter a valid 10-digit phone number.";
    document.getElementById("cust-phone").focus();
    return;
  }
  document.getElementById("phone-error").textContent = "";

  const orderId = `TB-${Date.now().toString().slice(-6)}`;
  const summary = cart.map((item) => `<p>${item.name} × ${item.quantity} — ₹${formatPrice(item.price * item.quantity)}</p>`).join("");
  const subtotal = cart.reduce((total, item) => total + item.price * item.quantity, 0);
  document.getElementById("order-id-display").textContent = `Order number: ${orderId}`;
  document.getElementById("order-summary-details").innerHTML = `${summary}<p><strong>Total including tax: ₹${formatPrice(subtotal * 1.05)}</strong></p>`;
  checkoutForm.reset();
  cart = [];
  renderCart();
  closeCart();
  orderModal.classList.add("show");
});

renderMenu();
renderCart();
