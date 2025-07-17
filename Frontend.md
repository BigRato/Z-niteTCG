## **2\. Frontend Básico**

**frontend/index.html**

\<\!DOCTYPE html\>  
\<html lang="pt-BR"\>  
\<head\>  
\<meta charset="UTF-8" /\>  
\<meta name="viewport" content="width=device-width, initial-scale=1" /\>  
\<title\>ZÊNITE TCG \- Loja Online\</title\>  
\<link rel="stylesheet" href="style.css" /\>  
\</head\>  
\<body\>  
  \<header\>  
    \<h1\>ZÊNITE TCG\</h1\>  
    \<input type="text" id="searchInput" placeholder="Buscar cartas..." /\>  
  \</header\>

  \<main\>  
    \<section id="catalog"\>\</section\>  
    \<aside id="cart"\>  
      \<h2\>Carrinho\</h2\>  
      \<ul id="cartItems"\>\</ul\>  
      \<p\>Total: R$ \<span id="totalPrice"\>0.00\</span\>\</p\>  
      \<button id="checkoutBtn"\>Finalizar Compra\</button\>  
    \</aside\>  
  \</main\>

  \<script src="script.js"\>\</script\>  
\</body\>  
\</html\>

frontend/style.css  
body {  
  font-family: Arial, sans-serif;  
  margin: 0;  
  display: flex;  
  flex-direction: column;  
  align-items: center;  
  background: \#f4f4f4;  
}

header {  
  width: 100%;  
  background: \#222;  
  color: white;  
  padding: 1rem;  
  text-align: center;  
}

\#searchInput {  
  margin-top: 10px;  
  padding: 0.5rem;  
  width: 300px;  
  font-size: 1rem;  
}

main {  
  display: flex;  
  max-width: 900px;  
  margin: 2rem auto;  
  gap: 2rem;  
  width: 100%;  
}

\#catalog {  
  flex: 3;  
  display: grid;  
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));  
  gap: 1rem;  
}

.card {  
  background: white;  
  padding: 1rem;  
  border-radius: 8px;  
  box-shadow: 0 0 6px rgba(0,0,0,0.1);  
}

\#cart {  
  flex: 1;  
  background: white;  
  padding: 1rem;  
  border-radius: 8px;  
  box-shadow: 0 0 6px rgba(0,0,0,0.1);  
  max-height: 400px;  
  overflow-y: auto;  
}

\#cartItems {  
  list-style: none;  
  padding: 0;  
}

\#cartItems li {  
  margin-bottom: 0.5rem;  
}

frontend/[script.js](http://script.js)  
const catalogEl \= document.getElementById('catalog');  
const cartItemsEl \= document.getElementById('cartItems');  
const totalPriceEl \= document.getElementById('totalPrice');  
const checkoutBtn \= document.getElementById('checkoutBtn');  
const searchInput \= document.getElementById('searchInput');

let catalog \= \[\];  
let cart \= \[\];

function fetchCatalog() {  
  fetch('http://localhost:3000/catalog')  
    .then(res \=\> res.json())  
    .then(data \=\> {  
      catalog \= data;  
      displayCatalog(catalog);  
    })  
    .catch(() \=\> {  
      catalogEl.innerHTML \= '\<p\>Não foi possível carregar o catálogo.\</p\>';  
    });  
}

function displayCatalog(items) {  
  catalogEl.innerHTML \= '';  
  items.forEach(item \=\> {  
    const card \= document.createElement('div');  
    card.className \= 'card';  
    card.innerHTML \= \`  
      \<h3\>${item.name}\</h3\>  
      \<p\>\<strong\>Tipo:\</strong\> ${item.type}\</p\>  
      \<p\>\<strong\>Cor:\</strong\> ${item.color}\</p\>  
      \<p\>\<strong\>Preço:\</strong\> R$ ${item.price.toFixed(2)}\</p\>  
      \<button onclick="addToCart(${item.id})"\>Adicionar ao Carrinho\</button\>  
    \`;  
    catalogEl.appendChild(card);  
  });  
}

function addToCart(id) {  
  const item \= catalog.find(card \=\> card.id \=== id);  
  if (\!item) return;

  const cartItem \= cart.find(c \=\> c.id \=== id);  
  if (cartItem) {  
    cartItem.qty++;  
  } else {  
    cart.push({ ...item, qty: 1 });  
  }  
  updateCart();  
}

function updateCart() {  
  cartItemsEl.innerHTML \= '';  
  let total \= 0;

  cart.forEach(item \=\> {  
    const li \= document.createElement('li');  
    li.textContent \= \`${item.name} x${item.qty} \- R$ ${(item.price \* item.qty).toFixed(2)}\`;  
    cartItemsEl.appendChild(li);  
    total \+= item.price \* item.qty;  
  });

  totalPriceEl.textContent \= total.toFixed(2);  
}

checkoutBtn.addEventListener('click', () \=\> {  
  if (cart.length \=== 0\) {  
    alert('Seu carrinho está vazio\!');  
    return;  
  }  
  alert('Compra finalizada\! Obrigado pela preferência.');  
  cart \= \[\];  
  updateCart();  
});

searchInput.addEventListener('input', () \=\> {  
  const term \= searchInput.value.toLowerCase();  
  const filtered \= catalog.filter(item \=\> item.name.toLowerCase().includes(term));  
  displayCatalog(filtered);  
});

fetchCatalog();

