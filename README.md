# task-4
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>All-in-One Project</title>

<style>
/* ===== GLOBAL ===== */
body {
  font-family: Arial;
  margin: 0;
  background: #f4f4f4;
}

header {
  background: #222;
  color: white;
  padding: 15px;
  text-align: center;
}

nav a {
  color: white;
  margin: 10px;
  text-decoration: none;
}

section {
  padding: 20px;
  margin: 15px;
  background: white;
  border-radius: 10px;
}

/* ===== TO-DO ===== */
input, select, button {
  padding: 10px;
  margin: 5px;
}

button {
  background: green;
  color: white;
  border: none;
  cursor: pointer;
}
button:hover {
  background: darkgreen;
}

/* ===== PRODUCTS ===== */
#productList {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.product {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 10px;
}

/* ===== RESPONSIVE ===== */
@media(max-width:600px){
  nav {
    display: flex;
    flex-direction: column;
  }
}
</style>

</head>
<body>

<header>
  <h1>My Advanced Project</h1>
  <nav>
    <a href="#about">About</a>
    <a href="#todo">To-Do</a>
    <a href="#products">Products</a>
    <a href="#contact">Contact</a>
  </nav>
</header>

<!-- ABOUT -->
<section id="about">
  <h2>About Me</h2>
  <p>I am a Web Developer skilled in HTML, CSS, and JavaScript.</p>
</section>

<!-- TO-DO APP -->
<section id="todo">
  <h2>To-Do List</h2>
  <input type="text" id="taskInput" placeholder="Enter task">
  <button onclick="addTask()">Add</button>
  <ul id="taskList"></ul>
</section>

<!-- PRODUCT PAGE -->
<section id="products">
  <h2>Product Listing</h2>

  <select id="filter" onchange="filterProducts()">
    <option value="all">All</option>
    <option value="electronics">Electronics</option>
    <option value="fashion">Fashion</option>
  </select>

  <select id="sort" onchange="sortProducts()">
    <option value="default">Sort</option>
    <option value="low">Price Low → High</option>
    <option value="high">Price High → Low</option>
  </select>

  <div id="productList"></div>
</section>

<!-- CONTACT -->
<section id="contact">
  <h2>Contact</h2>
  <p>Email: ashokkumarnaidu548@gmail.com</p>
</section>

<script>
// ===== TO-DO WITH LOCAL STORAGE =====
let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

function renderTasks() {
  const list = document.getElementById("taskList");
  list.innerHTML = "";

  tasks.forEach((task, index) => {
    const li = document.createElement("li");
    li.innerHTML = `${task} 
      <button onclick="deleteTask(${index})">❌</button>`;
    list.appendChild(li);
  });

  localStorage.setItem("tasks", JSON.stringify(tasks));
}

function addTask() {
  const input = document.getElementById("taskInput");
  if (input.value.trim()) {
    tasks.push(input.value);
    input.value = "";
    renderTasks();
  }
}

function deleteTask(index) {
  tasks.splice(index, 1);
  renderTasks();
}

renderTasks();

// ===== PRODUCT DATA =====
let products = [
  { name: "Laptop", category: "electronics", price: 50000 },
  { name: "Phone", category: "electronics", price: 20000 },
  { name: "Shirt", category: "fashion", price: 1000 },
  { name: "Shoes", category: "fashion", price: 3000 }
];

let currentProducts = [...products];

// DISPLAY PRODUCTS
function renderProducts(data) {
  const container = document.getElementById("productList");
  container.innerHTML = "";

  data.forEach(p => {
    const div = document.createElement("div");
    div.className = "product";
    div.innerHTML = `
      <h3>${p.name}</h3>
      <p>Category: ${p.category}</p>
      <p>Price: ₹${p.price}</p>
    `;
    container.appendChild(div);
  });
}

// FILTER
function filterProducts() {
  const value = document.getElementById("filter").value;
  currentProducts = value === "all"
    ? [...products]
    : products.filter(p => p.category === value);

  renderProducts(currentProducts);
}

// SORT
function sortProducts() {
  const value = document.getElementById("sort").value;

  if (value === "low") {
    currentProducts.sort((a, b) => a.price - b.price);
  } else if (value === "high") {
    currentProducts.sort((a, b) => b.price - a.price);
  }

  renderProducts(currentProducts);
}

// INITIAL LOAD
renderProducts(products);
</script>

</body>
</html>
