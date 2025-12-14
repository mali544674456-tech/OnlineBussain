# OnlineBussain
This website provides complete information about my business, including our services and contact details.
[index.HTML](https://github.com/user-attachments/files/24148086/index.HTML)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>OnlineBussian</title>

<style>
*{box-sizing:border-box}
body{margin:0;font-family:Segoe UI,Arial;background:#f1f3f6}

/* HEADER */
header{
background:#ff6a00;
color:#fff;
padding:30px 15px;
text-align:center;
}
header h1{margin:0;font-size:44px;font-weight:900}
header p{margin-top:6px;font-size:15px;opacity:.9}

/* ADD PRODUCT (ADMIN ONLY) */
.add{
display:none;
background:#fff;
max-width:520px;
margin:25px auto;
padding:20px;
border-radius:10px;
box-shadow:0 0 10px rgba(0,0,0,.15)
}
.add h3{text-align:center;margin-top:0}
.add input,.add textarea,.add button{width:100%;padding:10px;margin:6px 0}

/* PRODUCTS */
.products{
max-width:1200px;
margin:auto;
display:grid;
grid-template-columns:repeat(auto-fill,minmax(220px,1fr));
gap:15px;
padding:15px;
}
.card{background:#fff;border-radius:8px;box-shadow:0 2px 6px rgba(0,0,0,.1);text-align:center;padding-bottom:10px}
.card img{width:100%;height:180px;object-fit:cover;border-radius:8px 8px 0 0}
.card h3{margin:10px;font-size:16px}
.card p{margin:5px 10px}
.price{font-weight:bold;font-size:16px}
.card button{width:90%;padding:10px;margin:6px 0;background:#ff6a00;color:#fff;border:none;border-radius:4px;cursor:pointer}

/* ORDER */
.order{display:none;background:#fff;max-width:500px;margin:20px auto;padding:20px;border-radius:10px}
.order input,.order select,.order button{width:100%;padding:10px;margin:6px 0}

/* FOOTER */
footer{background:#111;color:#ccc;text-align:center;padding:30px;margin-top:30px}
</style>
</head>

<body>

<header>
<h1>OnlineBussian</h1>
<p>Buy & Sell with Trust</p>
</header>

<!-- ADMIN ADD PRODUCT -->
<div class="add" id="adminBox">
<h3>Add Product (Admin Only)</h3>
<input id="pname" placeholder="Product Name">
<input id="pprice" type="number" placeholder="Price (PKR)">
<textarea id="pdesc" placeholder="Product Description"></textarea>
<input id="pimage" type="file" accept="image/*">
<button onclick="addProduct()">Add Product</button>
</div>

<div class="products" id="productContainer"></div>

<div class="order" id="orderBox">
<h3>Order Details</h3>
<form onsubmit="sendOrder(event)">
<input id="cname" placeholder="Customer Name" required>
<input id="cphone" placeholder="Phone Number" required>
<input id="caddress" placeholder="Full Address" required>
<select id="cpayment" required>
<option value="">Select Payment Method</option>
<option>Cash on Delivery</option>
<option>JazzCash</option>
<option>Easypaisa</option>
</select>
<button type="submit">Submit Order</button>
</form>
</div>

<footer>
<p>📧 Email: <b>m.faizan7857@gmail.com</b></p>
<p>📞 Phone / WhatsApp: <b>03435860103</b></p>
<p>💳 JazzCash: <b>03054860106</b></p>
<p>💳 Easypaisa: <b>03435860106</b></p>
<p>© 2025 OnlineBussian</p>
</footer>

<script>
/******** ADMIN ACCESS (ONLY YOUR COMPUTER) ********/
let isAdmin = localStorage.getItem("isAdmin");
if (window.location.search.includes("admin")) {
  localStorage.setItem("isAdmin", "yes");
  isAdmin = "yes";
}
if (isAdmin === "yes") {
  document.getElementById("adminBox").style.display = "block";
}

/******** PRODUCTS ********/
let products = JSON.parse(localStorage.getItem("products") || "[]");
let selectedProduct = "";

function addProduct(){
  if(!pname.value || !pprice.value || !pdesc.value || !pimage.files[0]){
    alert("Fill all fields");
    return;
  }
  const r = new FileReader();
  r.onload = () => {
    products.push({
      name: pname.value,
      price: parseInt(pprice.value),
      desc: pdesc.value,
      img: r.result
    });
    localStorage.setItem("products", JSON.stringify(products));
    render();
  };
  r.readAsDataURL(pimage.files[0]);

  pname.value = pprice.value = pdesc.value = pimage.value = "";
}

function render(){
  productContainer.innerHTML = "";
  products.forEach(p => {
    productContainer.innerHTML += `
    <div class="card">
      <img src="${p.img}">
      <h3>${p.name}</h3>
      <p>${p.desc}</p>
      <p class="price">Rs ${p.price}</p>
      <button onclick="buy('${p.name}','${p.price}')">Buy Now</button>
    </div>`;
  });
}

function buy(name, price){
  selectedProduct = `Product: ${name} | Price: Rs ${price}`;
  orderBox.style.display = "block";
  window.scrollTo(0, document.body.scrollHeight);
}

function sendOrder(e){
  e.preventDefault();
  const body = `${selectedProduct}\nName: ${cname.value}\nPhone: ${cphone.value}\nAddress: ${caddress.value}\nPayment: ${cpayment.value}`;
  window.location = "mailto:m.faizan7857@gmail.com?subject=New Order&body=" + encodeURIComponent(body);
}

render();
</script>

</body>
</html>
