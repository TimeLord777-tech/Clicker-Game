# Clicker-Game
<!DOCTYPE html>
<html>
<head>
    <title>Animal Clicker Adventure</title>
    <style>
        body {
            font-family: 'Comic Sans MS', cursive, sans-serif;
            background-color: #f0f8ff;
            text-align: center;
            padding: 20px;
        }
        #game-container {
            max-width: 800px;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 0 15px rgba(0,0,0,0.1);
        }
        #click-area {
            background-color: #e6f7ff;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
            cursor: pointer;
            transition: transform 0.1s;
        }
        #click-area:active {
            transform: scale(0.95);
        }
        #animals-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 15px;
            margin-bottom: 20px;
        }
        .animal {
            width: 120px;
            padding: 10px;
            background-color: #fffacd;
            border-radius: 10px;
            position: relative;
        }
        .animal img {
            width: 100px;
            height: 100px;
            object-fit: contain;
        }
        .accessory {
            position: absolute;
            width: 40px;
            height: 40px;
        }
        #shop {
            background-color: #f0fff0;
            padding: 15px;
            border-radius: 10px;
        }
        .shop-item {
            display: inline-block;
            margin: 10px;
            padding: 10px;
            background-color: white;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s;
        }
        .shop-item:hover {
            transform: scale(1.05);
            box-shadow: 0 0 10px rgba(0,0,0,0.2);
        }
        .shop-item img {
            width: 50px;
            height: 50px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
        }
        button:hover {
            background-color: #45a049;
        }
        #stats {
            margin-top: 20px;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <div id="game-container">
        <h1>🐾 Animal Clicker Adventure 🐾</h1>
        
        <div id="stats">
            <p>Clicks: <span id="click-count">0</span></p>
            <p>Animals: <span id="animal-count">0</span></p>
        </div>
        
        <div id="click-area">
            <h2>Click to Collect Animals!</h2>
            <img src="https://cdn-icons-png.flaticon.com/512/616/616408.png" width="100">
        </div>
        
        <button id="buy-animal">Buy New Animal (100 clicks)</button>
        
        <div id="animals-container">
            <!-- Animals will appear here -->
        </div>
        
        <div id="shop">
            <h2>🐶 Animal Accessories Shop 🐱</h2>
            <div id="shop-items">
                <!-- Shop items will appear here -->
            </div>
        </div>
    </div>

    <script>
        // Game state
        let clicks = 0;
        let animals = [];
        let accessories = [
            { id: 1, name: "Top Hat", price: 50, img: "https://cdn-icons-png.flaticon.com/512/2345/2345277.png", position: "top:-20px; left:30px" },
            { id: 2, name: "Bow Tie", price: 30, img: "https://cdn-icons-png.flaticon.com/512/3048/3048127.png", position: "top:70px; left:30px" },
            { id: 3, name: "Sunglasses", price: 40, img: "https://cdn-icons-png.flaticon.com/512/892/892708.png", position: "top:30px; left:30px" },
            { id: 4, name: "Crown", price: 100, img: "https://cdn-icons-png.flaticon.com/512/2583/2583344.png", position: "top:-25px; left:30px" },
            { id: 5, name: "Scarf", price: 60, img: "https://cdn-icons-png.flaticon.com/512/5997/5997625.png", position: "top:80px; left:30px" }
        ];
        
        // DOM elements
        const clickArea = document.getElementById('click-area');
        const clickCount = document.getElementById('click-count');
        const animalCount = document.getElementById('animal-count');
        const animalsContainer = document.getElementById('animals-container');
        const buyAnimalBtn = document.getElementById('buy-animal');
        const shopItems = document.getElementById('shop-items');
        
        // Initialize shop
        function initShop() {
            accessories.forEach(accessory => {
                const item = document.createElement('div');
                item.className = 'shop-item';
                item.innerHTML = `
                    <img src="${accessory.img}" alt="${accessory.name}">
                    <p>${accessory.name}</p>
                    <p>${accessory.price} clicks</p>
                `;
                item.addEventListener('click', () => buyAccessory(accessory));
                shopItems.appendChild(item);
            });
        }
        
        // Click handler
        clickArea.addEventListener('click', () => {
            clicks++;
            clickCount.textContent = clicks;
            
            // Animate click
            clickArea.style.transform = 'scale(0.95)';
            setTimeout(() => {
                clickArea.style.transform = 'scale(1)';
            }, 100);
            
            // Random chance to get a free animal (5% chance)
            if (Math.random() < 0.05 && animals.length < 10) {
                addAnimal();
            }
        });
        
        // Buy animal
        buyAnimalBtn.addEventListener('click', () => {
            if (clicks >= 100) {
                clicks -= 100;
                clickCount.textContent = clicks;
                addAnimal();
            } else {
                alert("Not enough clicks! You need 100 clicks to buy an animal.");
            }
        });
        
        // Add a new animal
        function addAnimal() {
            const animalTypes = [
                { name: "Dog", img: "https://cdn-icons-png.flaticon.com/512/616/616408.png" },
                { name: "Cat", img: "https://cdn-icons-png.flaticon.com/512/616/616430.png" },
                { name: "Rabbit", img: "https://cdn-icons-png.flaticon.com/512/3069/3069172.png" },
                { name: "Fox", img: "https://cdn-icons-png.flaticon.com/512/616/616516.png" },
                { name: "Panda", img: "https://cdn-icons-png.flaticon.com/512/616/616494.png" }
            ];
            
            const randomType = animalTypes[Math.floor(Math.random() * animalTypes.length)];
            const animal = {
                id: animals.length + 1,
                type: randomType.name,
                img: randomType.img,
                accessories: []
            };
            
            animals.push(animal);
            animalCount.textContent = animals.length;
            
            renderAnimals();
        }
        
        // Buy accessory
        function buyAccessory(accessory) {
            if (clicks >= accessory.price && animals.length > 0) {
                clicks -= accessory.price;
                clickCount.textContent = clicks;
                
                // Add accessory to a random animal
                const randomAnimalIndex = Math.floor(Math.random() * animals.length);
                animals[randomAnimalIndex].accessories.push(accessory);
                
                renderAnimals();
            } else if (animals.length === 0) {
                alert("You need at least one animal to buy accessories!");
            } else {
                alert(`Not enough clicks! You need ${accessory.price} clicks to buy this accessory.`);
            }
        }
        
        // Render all animals
        function renderAnimals() {
            animalsContainer.innerHTML = '';
            
            animals.forEach(animal => {
                const animalElement = document.createElement('div');
                animalElement.className = 'animal';
                animalElement.innerHTML = `
                    <h3>${animal.type}</h3>
                    <img src="${animal.img}" alt="${animal.type}">
                `;
                
                // Add accessories
                animal.accessories.forEach(acc => {
                    const accElement = document.createElement('img');
                    accElement.className = 'accessory';
                    accElement.src = acc.img;
                    accElement.alt = acc.name;
                    accElement.style = acc.position;
                    animalElement.appendChild(accElement);
                });
                
                animalsContainer.appendChild(animalElement);
            });
        }
        
        // Initialize game
        initShop();
    </script>
</body>
</html>
