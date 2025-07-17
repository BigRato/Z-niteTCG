## **1\. Backend Simples (Node.js \+ JSON)**

**backend/catalog.json** (exemplo de catálogo simples):

\[  
  {  
    "id": 1,  
    "name": "Luffy, Capitão dos Chapéus de Palha",  
    "type": "Personagem",  
    "color": "Vermelho",  
    "price": 15.00,  
    "rarity": "Raro",  
    "description": "Carta poderosa do protagonista."  
  },  
  {  
    "id": 2,  
    "name": "Zoro, Espadachim",  
    "type": "Personagem",  
    "color": "Verde",  
    "price": 12.00,  
    "rarity": "Comum",  
    "description": "Especialista em espadas."  
  },  
  {  
    "id": 3,  
    "name": "Big Mom, Yonko",  
    "type": "Personagem",  
    "color": "Rosa",  
    "price": 20.00,  
    "rarity": "Muito Raro",  
    "description": "Uma das quatro imperatrizes do mar."  
  }  
\]

**backend/server.js** (servidor simples para servir o JSON):  
const express \= require('express');  
const app \= express();  
const port \= 3000;  
const catalog \= require('./catalog.json');

app.get('/catalog', (req, res) \=\> {  
  res.json(catalog);  
});

app.listen(port, () \=\> {  
  console.log(\`Servidor backend rodando em http://localhost:${port}\`);  
});

