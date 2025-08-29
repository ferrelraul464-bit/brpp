<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Restaurante - Menú Digital</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            min-height: 100vh;
            box-shadow: 0 0 20px rgba(0,0,0,0.1);
        }

        .header {
            background: linear-gradient(135deg, #ff6b6b, #ee5a24);
            color: white;
            padding: 2rem;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .header::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="50" r="2" fill="rgba(255,255,255,0.1)"/></svg>') repeat;
            animation: float 20s linear infinite;
            pointer-events: none;
        }

        @keyframes float {
            0% { transform: translate(-50%, -50%) rotate(0deg); }
            100% { transform: translate(-50%, -50%) rotate(360deg); }
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 0.5rem;
            position: relative;
            z-index: 1;
        }

        .header p {
            font-size: 1.2rem;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .admin-toggle {
            position: fixed;
            top: 20px;
            left: 20px;
            background: #6c5ce7;
            color: white;
            border: none;
            border-radius: 25px;
            padding: 0.8rem 1.2rem;
            font-size: 0.9rem;
            cursor: pointer;
            z-index: 1000;
            transition: all 0.3s ease;
        }

        .admin-toggle:hover {
            background: #5f3dc4;
            transform: scale(1.05);
        }

        .cart-icon {
            position: fixed;
            top: 20px;
            right: 20px;
            background: #ff6b6b;
            color: white;
            border: none;
            border-radius: 50%;
            width: 60px;
            height: 60px;
            font-size: 1.5rem;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(255, 107, 107, 0.3);
            z-index: 1000;
            transition: all 0.3s ease;
        }

        .cart-icon:hover {
            transform: scale(1.1);
            box-shadow: 0 6px 20px rgba(255, 107, 107, 0.4);
        }

        .cart-count {
            position: absolute;
            top: -8px;
            right: -8px;
            background: #27ae60;
            color: white;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            font-size: 0.8rem;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }

        .admin-panel {
            position: fixed;
            top: 0;
            left: -400px;
            width: 400px;
            height: 100vh;
            background: white;
            box-shadow: 2px 0 20px rgba(0,0,0,0.1);
            z-index: 1500;
            transition: left 0.3s ease;
            overflow-y: auto;
        }

        .admin-panel.active {
            left: 0;
        }

        .admin-header {
            background: #6c5ce7;
            color: white;
            padding: 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .admin-content {
            padding: 1.5rem;
        }

        .admin-section {
            margin-bottom: 2rem;
        }

        .admin-section h3 {
            color: #2c3e50;
            margin-bottom: 1rem;
            padding-bottom: 0.5rem;
            border-bottom: 2px solid #ecf0f1;
        }

        .form-group {
            margin-bottom: 1rem;
        }

        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            color: #2c3e50;
            font-weight: 600;
        }

        .form-group input, .form-group textarea, .form-group select {
            width: 100%;
            padding: 0.8rem;
            border: 2px solid #ecf0f1;
            border-radius: 10px;
            font-size: 1rem;
            transition: border-color 0.3s ease;
        }

        .form-group input:focus, .form-group textarea:focus, .form-group select:focus {
            outline: none;
            border-color: #6c5ce7;
        }

        .btn {
            background: #6c5ce7;
            color: white;
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 25px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            margin-right: 0.5rem;
            margin-bottom: 0.5rem;
        }

        .btn:hover {
            background: #5f3dc4;
            transform: translateY(-2px);
        }

        .btn-danger {
            background: #e74c3c;
        }

        .btn-danger:hover {
            background: #c0392b;
        }

        .btn-success {
            background: #27ae60;
        }

        .btn-success:hover {
            background: #219a52;
        }

        .categories {
            display: flex;
            overflow-x: auto;
            background: white;
            padding: 1rem;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .category-btn {
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            padding: 0.8rem 1.5rem;
            margin: 0 0.5rem;
            border-radius: 25px;
            cursor: pointer;
            white-space: nowrap;
            font-weight: 600;
            transition: all 0.3s ease;
            color: #495057;
        }

        .category-btn:hover, .category-btn.active {
            background: #ff6b6b;
            border-color: #ff6b6b;
            color: white;
            transform: translateY(-2px);
        }

        .menu-section {
            padding: 2rem;
        }

        .category-title {
            font-size: 2rem;
            margin-bottom: 1.5rem;
            color: #2c3e50;
            text-align: center;
            position: relative;
        }

        .category-title::after {
            content: '';
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 60px;
            height: 3px;
            background: linear-gradient(135deg, #ff6b6b, #ee5a24);
            border-radius: 2px;
        }

        .menu-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1.5rem;
            margin-bottom: 3rem;
        }

        .menu-item {
            background: white;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 5px 20px rgba(0,0,0,0.1);
            transition: all 0.3s ease;
            border: 2px solid transparent;
            position: relative;
        }

        .menu-item:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(0,0,0,0.15);
            border-color: #ff6b6b;
        }

        .edit-item-btn {
            position: absolute;
            top: 10px;
            right: 10px;
            background: rgba(108, 92, 231, 0.9);
            color: white;
            border: none;
            border-radius: 50%;
            width: 35px;
            height: 35px;
            font-size: 1rem;
            cursor: pointer;
            z-index: 10;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .menu-item:hover .edit-item-btn {
            opacity: 1;
        }

        .item-image {
            height: 200px;
            background: linear-gradient(135deg, #ffeaa7, #fab1a0);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
            color: white;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            background-size: cover;
            background-position: center;
        }

        .item-content {
            padding: 1.5rem;
        }

        .item-name {
            font-size: 1.3rem;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 0.5rem;
        }

        .item-description {
            color: #7f8c8d;
            margin-bottom: 1rem;
            line-height: 1.5;
        }

        .item-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .item-price {
            font-size: 1.4rem;
            font-weight: bold;
            color: #27ae60;
        }

        .add-btn {
            background: linear-gradient(135deg, #00b894, #00a085);
            color: white;
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 25px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .add-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 184, 148, 0.3);
        }

        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 2000;
        }

        .modal-content {
            background: white;
            border-radius: 20px;
            padding: 2rem;
            max-width: 500px;
            width: 90%;
            max-height: 80vh;
            overflow-y: auto;
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
            padding-bottom: 1rem;
            border-bottom: 2px solid #ecf0f1;
        }

        .close-btn {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: #95a5a6;
        }

        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem 0;
            border-bottom: 1px solid #ecf0f1;
        }

        .cart-item-info h4 {
            color: #2c3e50;
            margin-bottom: 0.5rem;
        }

        .cart-item-price {
            color: #27ae60;
            font-weight: bold;
        }

        .quantity-controls {
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .quantity-btn {
            background: #ff6b6b;
            color: white;
            border: none;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .cart-total {
            text-align: center;
            margin: 1.5rem 0;
            font-size: 1.5rem;
            font-weight: bold;
            color: #2c3e50;
        }

        .whatsapp-btn {
            background: #25D366;
            color: white;
            border: none;
            padding: 1rem 2rem;
            border-radius: 25px;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
            margin-top: 1rem;
            transition: all 0.3s ease;
        }

        .whatsapp-btn:hover {
            background: #20b358;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(37, 211, 102, 0.3);
        }

        .empty-cart {
            text-align: center;
            color: #95a5a6;
            font-size: 1.2rem;
            padding: 2rem;
        }

        .image-upload {
            position: relative;
            display: inline-block;
            margin-bottom: 1rem;
        }

        .image-preview {
            width: 100px;
            height: 100px;
            border: 2px dashed #ddd;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            overflow: hidden;
            background-size: cover;
            background-position: center;
        }

        .item-list {
            max-height: 300px;
            overflow-y: auto;
            border: 1px solid #ecf0f1;
            border-radius: 10px;
            padding: 1rem;
        }

        .item-list-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0.8rem;
            margin-bottom: 0.5rem;
            background: #f8f9fa;
            border-radius: 8px;
        }

        .restaurant-info {
            text-align: center;
            margin-bottom: 2rem;
        }

        .share-link {
            background: #f8f9fa;
            border: 2px solid #ecf0f1;
            border-radius: 10px;
            padding: 1rem;
            margin-top: 1rem;
            text-align: center;
        }

        .share-link input {
            width: 100%;
            margin-bottom: 0.5rem;
        }

        @media (max-width: 768px) {
            .admin-panel {
                width: 100%;
                left: -100%;
            }
            
            .header h1 {
                font-size: 2rem;
            }
            
            .menu-grid {
                grid-template-columns: 1fr;
            }
            
            .modal-content {
                width: 95%;
                padding: 1rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <button class="admin-toggle" onclick="toggleAdmin()">⚙️ Admin</button>
        
        <header class="header">
            <h1 id="restaurantName">🍕 Mi Restaurante</h1>
            <p id="restaurantDescription">Deliciosa comida a domicilio</p>
        </header>

        <button class="cart-icon" onclick="toggleCart()">
            🛒
            <span class="cart-count" id="cartCount">0</span>
        </button>

        <div class="categories" id="categoriesContainer">
            <!-- Las categorías se generan dinámicamente -->
        </div>

        <div class="menu-section" id="menuContainer">
            <!-- El menú se genera dinámicamente -->
        </div>
    </div>

    <!-- Panel de Administración -->
    <div class="admin-panel" id="adminPanel">
        <div class="admin-header">
            <h2>⚙️ Panel Admin</h2>
            <button class="close-btn" onclick="toggleAdmin()">&times;</button>
        </div>
        <div class="admin-content">
            <!-- Configuración del Restaurante -->
            <div class="admin-section">
                <h3>🏪 Info del Restaurante</h3>
                <div class="form-group">
                    <label>Nombre del restaurante:</label>
                    <input type="text" id="adminRestaurantName" onchange="updateRestaurantInfo()">
                </div>
                <div class="form-group">
                    <label>Descripción:</label>
                    <input type="text" id="adminRestaurantDescription" onchange="updateRestaurantInfo()">
                </div>
                <div class="form-group">
                    <label>Número de WhatsApp (con código de país):</label>
                    <input type="text" id="adminWhatsappNumber" placeholder="5491123456789">
                </div>
                <div class="share-link">
                    <strong>🔗 Link para compartir:</strong>
                    <input type="text" id="shareLink" readonly onclick="this.select()">
                    <button class="btn" onclick="copyLink()">📋 Copiar Link</button>
                    <button class="btn" onclick="shareWhatsApp()">📱 Compartir por WhatsApp</button>
                </div>
            </div>

            <!-- Gestión de Categorías -->
            <div class="admin-section">
                <h3>📂 Categorías</h3>
                <div class="form-group">
                    <label>Nueva categoría:</label>
                    <input type="text" id="newCategoryName" placeholder="Ej: Pizzas">
                </div>
                <button class="btn btn-success" onclick="addCategory()">➕ Agregar Categoría</button>
                <div class="item-list" id="categoriesList">
                    <!-- Lista de categorías -->
                </div>
            </div>

            <!-- Gestión de Productos -->
            <div class="admin-section">
                <h3>🍽️ Productos</h3>
                <div class="form-group">
                    <label>Categoría:</label>
                    <select id="productCategory">
                        <!-- Se llena dinámicamente -->
                    </select>
                </div>
                <div class="form-group">
                    <label>Nombre del producto:</label>
                    <input type="text" id="productName">
                </div>
                <div class="form-group">
                    <label>Descripción:</label>
                    <textarea id="productDescription" rows="3"></textarea>
                </div>
                <div class="form-group">
                    <label>Precio:</label>
                    <input type="number" id="productPrice" step="0.01" min="0">
                </div>
                <div class="form-group">
                    <label>Emoji o imagen:</label>
                    <div class="image-upload">
                        <div class="image-preview" id="productImagePreview" onclick="document.getElementById('productImage').click()">
                            <input type="file" id="productImage" accept="image/*" style="display: none;" onchange="previewImage()">
                            <span>📷</span>
                        </div>
                    </div>
                    <input type="text" id="productEmoji" placeholder="O ingresa un emoji: 🍕">
                </div>
                <button class="btn btn-success" onclick="addProduct()">➕ Agregar Producto</button>
                <button class="btn" onclick="clearProductForm()">🗑️ Limpiar</button>
                <div class="item-list" id="productsList">
                    <!-- Lista de productos -->
                </div>
            </div>

            <!-- Datos guardados -->
            <div class="admin-section">
                <h3>💾 Datos</h3>
                <button class="btn btn-success" onclick="exportData()">📤 Exportar Datos</button>
                <button class="btn" onclick="importData()">📥 Importar Datos</button>
                <input type="file" id="importFile" accept=".json" style="display: none;" onchange="handleImport()">
                <button class="btn btn-danger" onclick="resetData()">🗑️ Resetear Todo</button>
            </div>
        </div>
    </div>

    <!-- Modal del Carrito -->
    <div class="modal" id="cartModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>🛒 Tu Pedido</h2>
                <button class="close-btn" onclick="toggleCart()">&times;</button>
            </div>
            <div id="cartItems"></div>
            <div class="cart-total" id="cartTotal">Total: $0.00</div>
            
            <div id="customerForm" style="display: none;">
                <h3>Datos para el pedido:</h3>
                <div class="form-group">
                    <label for="customerName">Nombre completo:</label>
                    <input type="text" id="customerName" required>
                </div>
                <div class="form-group">
                    <label for="customerPhone">Teléfono:</label>
                    <input type="tel" id="customerPhone" required>
                </div>
                <div class="form-group">
                    <label for="customerAddress">Dirección de entrega:</label>
                    <textarea id="customerAddress" rows="3" required></textarea>
                </div>
                <div class="form-group">
                    <label for="customerNotes">Notas adicionales (opcional):</label>
                    <textarea id="customerNotes" rows="2" placeholder="Alguna aclaración sobre tu pedido..."></textarea>
                </div>
                <button class="whatsapp-btn" onclick="sendWhatsApp()">📱 Enviar Pedido por WhatsApp</button>
            </div>
        </div>
    </div>

    <!-- Modal de Edición de Producto -->
    <div class="modal" id="editModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>✏️ Editar Producto</h2>
                <button class="close-btn" onclick="closeEditModal()">&times;</button>
            </div>
            <div class="form-group">
                <label>Nombre:</label>
                <input type="text" id="editProductName">
            </div>
            <div class="form-group">
                <label>Descripción:</label>
                <textarea id="editProductDescription" rows="3"></textarea>
            </div>
            <div class="form-group">
                <label>Precio:</label>
                <input type="number" id="editProductPrice" step="0.01" min="0">
            </div>
            <div class="form-group">
                <label>Emoji:</label>
                <input type="text" id="editProductEmoji">
            </div>
            <button class="btn btn-success" onclick="saveProductEdit()">💾 Guardar</button>
            <button class="btn btn-danger" onclick="deleteProduct()">🗑️ Eliminar</button>
        </div>
    </div>

    <script>
        // Datos del sistema
        let restaurantData = {
            name: "🍕 Mi Restaurante",
            description: "Deliciosa comida a domicilio",
            whatsappNumber: "1234567890",
            categories: ["Entradas", "Principales", "Bebidas", "Postres"],
            products: [
                {
                    id: 1,
                    name: "Ensalada César",
                    description: "Fresca ensalada con lechuga, crutones, parmesano y aderezo césar",
                    price: 12.50,
                    category: "Entradas",
                    emoji: "🥗",
                    image: null
                },
                {
                    id: 2,
                    name: "Pizza Margherita",
                    description: "Pizza clásica con tomate, mozzarella fresca y albahaca",
                    price: 22.00,
                    category: "Principales",
                    emoji: "🍕",
                    image: null
                },
                {
                    id: 3,
                    name: "Coca Cola",
                    description: "Refresco clásico bien frío (500ml)",
                    price: 3.50,
                    category: "Bebidas",
                    emoji: "🥤",
                    image: null
                },
                {
                    id: 4,
                    name: "Tiramisu",
                    description: "Delicioso postre italiano con café y mascarpone",
                    price: 8.50,
                    category: "Postres",
                    emoji: "🍰",
                    image: null
                }
            ]
        };

        let cart = [];
        let editingProductId = null;

        // Cargar datos del localStorage al iniciar
        function loadData() {
            const saved = localStorage.getItem('restaurantData');
            if (saved) {
                restaurantData = JSON.parse(saved);
            }
            updateDisplay();
            updateShareLink();
        }

        // Guardar datos en localStorage
        function saveData() {
            localStorage.setItem('restaurantData', JSON.stringify(restaurantData));
        }

        // Actualizar toda la interfaz
        function updateDisplay() {
            updateRestaurantInfo();
            updateCategories();
            updateMenu();
            updateAdminPanel();
        }

        // Actualizar información del restaurante
        function updateRestaurantInfo() {
            const name = document.getElementById('adminRestaurantName').value || restaurantData.name;
            const description = document.getElementById('adminRestaurantDescription').value || restaurantData.description;
            
            restaurantData.name = name;
            restaurantData.description = description;
            
            document.getElementById('restaurantName').textContent = name;
            document.getElementById('restaurantDescription').textContent = description;
            document.getElementById('adminRestaurantName').value = name;
            document.getElementById('adminRestaurantDescription').value = description;
            
            saveData();
        }

        // Actualizar categorías
        function updateCategories() {
            const container = document.getElementById('categoriesContainer');
            const adminSelect = document.getElementById('productCategory');
            const categoriesList = document.getElementById('categoriesList');
            
            // Botones de categorías
            container.innerHTML = '<button class="category-btn active" onclick="showCategory(\'todas\')">Todas</button>';
            restaurantData.categories.forEach(category => {
                container.innerHTML += `<button class="category-btn" onclick="showCategory('${category}')">${category}</button>`;
            });

            // Select del admin
            adminSelect.innerHTML = '';
            restaurantData.categories.forEach(category => {
                adminSelect.innerHTML += `<option value="${category}">${category}</option>`;
            });

            // Lista del admin
            categoriesList.innerHTML = '';
            restaurantData.categories.forEach((category, index) => {
                categoriesList.innerHTML += `
                    <div class="item-list-item">
                        <span>${category}</span>
                        <button class="btn btn-danger" onclick="deleteCategory(${index})">🗑️</button>
                    </div>
                `;
            });
        }

        // Actualizar menú
        function updateMenu() {
            const container = document.getElementById('menuContainer');
            container.innerHTML = '';

            restaurantData.categories.forEach(category => {
                const categoryProducts = restaurantData.products.filter(p => p.category === category);
                if (categoryProducts.length === 0) return;

                container.innerHTML += `
                    <div id="${category}" class="category-section">
                        <h2 class="category-title">${category}</h2>
                        <div class="menu-grid">
                            ${categoryProducts.map(product => `
                                <div class="menu-item">
                                    <button class="edit-item-btn" onclick="editProduct(${product.id})">✏️</button>
                                    <div class="item-image" ${product.image ? `style="background-image: url(${product.image})"` : ''}>
                                        ${!product.image ? product.emoji : ''}
                                    </div>
                                    <div class="item-content">
                                        <h3 class="item-name">${product.name}</h3>
                                        <p class="item-description">${product.description}</p>
                                        <div class="item-footer">
                                            <span class="item-price">$${product.price.toFixed(2)}</span>
                                            <button class="add-btn" onclick="addToCart('${product.name}', ${product.price})">Agregar</button>
                                        </div>
                                    </div>
                                </div>
                            `).join('')}
                        </div>
                    </div>
                `;
            });

            updateProductsList();
        }

        // Actualizar panel de administración
        function updateAdminPanel() {
            document.getElementById('adminRestaurantName').value = restaurantData.name;
            document.getElementById('adminRestaurantDescription').value = restaurantData.description;
            document.getElementById('adminWhatsappNumber').value = restaurantData.whatsappNumber;
        }

        // Actualizar lista de productos en admin
        function updateProductsList() {
            const productsList = document.getElementById('productsList');
            productsList.innerHTML = '';
            
            restaurantData.products.forEach(product => {
                productsList.innerHTML += `
                    <div class="item-list-item">
                        <div>
                            <strong>${product.name}</strong><br>
                            <small>${product.category} - ${product.price.toFixed(2)}</small>
                        </div>
                        <div>
                            <button class="btn" onclick="editProduct(${product.id})">✏️</button>
                            <button class="btn btn-danger" onclick="confirmDeleteProduct(${product.id})">🗑️</button>
                        </div>
                    </div>
                `;
            });
        }

        // Mostrar/ocultar categorías
        function showCategory(category) {
            // Actualizar botones activos
            document.querySelectorAll('.category-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');

            // Mostrar/ocultar secciones
            const sections = document.querySelectorAll('.category-section');
            sections.forEach(section => {
                if (category === 'todas' || section.id === category) {
                    section.style.display = 'block';
                } else {
                    section.style.display = 'none';
                }
            });
        }

        // Toggle panel de administración
        function toggleAdmin() {
            const panel = document.getElementById('adminPanel');
            panel.classList.toggle('active');
        }

        // Agregar nueva categoría
        function addCategory() {
            const name = document.getElementById('newCategoryName').value.trim();
            if (name && !restaurantData.categories.includes(name)) {
                restaurantData.categories.push(name);
                document.getElementById('newCategoryName').value = '';
                updateCategories();
                saveData();
            } else {
                alert('La categoría ya existe o el nombre está vacío');
            }
        }

        // Eliminar categoría
        function deleteCategory(index) {
            if (confirm('¿Eliminar esta categoría? Los productos se mantendrán.')) {
                restaurantData.categories.splice(index, 1);
                updateCategories();
                saveData();
            }
        }

        // Previsualizar imagen
        function previewImage() {
            const file = document.getElementById('productImage').files[0];
            const preview = document.getElementById('productImagePreview');
            
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    preview.style.backgroundImage = `url(${e.target.result})`;
                    preview.innerHTML = '';
                };
                reader.readAsDataURL(file);
            }
        }

        // Agregar nuevo producto
        function addProduct() {
            const name = document.getElementById('productName').value.trim();
            const description = document.getElementById('productDescription').value.trim();
            const price = parseFloat(document.getElementById('productPrice').value);
            const category = document.getElementById('productCategory').value;
            const emoji = document.getElementById('productEmoji').value.trim();
            const imageFile = document.getElementById('productImage').files[0];

            if (!name || !description || !price || !category) {
                alert('Por favor completa todos los campos obligatorios');
                return;
            }

            const newProduct = {
                id: Date.now(),
                name,
                description,
                price,
                category,
                emoji: emoji || '🍽️',
                image: null
            };

            if (imageFile) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    newProduct.image = e.target.result;
                    restaurantData.products.push(newProduct);
                    updateMenu();
                    clearProductForm();
                    saveData();
                };
                reader.readAsDataURL(imageFile);
            } else {
                restaurantData.products.push(newProduct);
                updateMenu();
                clearProductForm();
                saveData();
            }
        }

        // Limpiar formulario de producto
        function clearProductForm() {
            document.getElementById('productName').value = '';
            document.getElementById('productDescription').value = '';
            document.getElementById('productPrice').value = '';
            document.getElementById('productEmoji').value = '';
            document.getElementById('productImage').value = '';
            document.getElementById('productImagePreview').style.backgroundImage = '';
            document.getElementById('productImagePreview').innerHTML = '<span>📷</span>';
        }

        // Editar producto
        function editProduct(productId) {
            const product = restaurantData.products.find(p => p.id === productId);
            if (!product) return;

            editingProductId = productId;
            document.getElementById('editProductName').value = product.name;
            document.getElementById('editProductDescription').value = product.description;
            document.getElementById('editProductPrice').value = product.price;
            document.getElementById('editProductEmoji').value = product.emoji;
            
            document.getElementById('editModal').style.display = 'flex';
        }

        // Guardar edición de producto
        function saveProductEdit() {
            const product = restaurantData.products.find(p => p.id === editingProductId);
            if (!product) return;

            product.name = document.getElementById('editProductName').value.trim();
            product.description = document.getElementById('editProductDescription').value.trim();
            product.price = parseFloat(document.getElementById('editProductPrice').value);
            product.emoji = document.getElementById('editProductEmoji').value.trim();

            updateMenu();
            closeEditModal();
            saveData();
        }

        // Eliminar producto
        function deleteProduct() {
            if (confirm('¿Eliminar este producto?')) {
                restaurantData.products = restaurantData.products.filter(p => p.id !== editingProductId);
                updateMenu();
                closeEditModal();
                saveData();
            }
        }

        // Confirmar eliminación de producto
        function confirmDeleteProduct(productId) {
            if (confirm('¿Eliminar este producto?')) {
                restaurantData.products = restaurantData.products.filter(p => p.id !== productId);
                updateMenu();
                saveData();
            }
        }

        // Cerrar modal de edición
        function closeEditModal() {
            document.getElementById('editModal').style.display = 'none';
            editingProductId = null;
        }

        // Actualizar número de WhatsApp
        function updateWhatsAppNumber() {
            restaurantData.whatsappNumber = document.getElementById('adminWhatsappNumber').value;
            saveData();
        }

        // Generar y actualizar link para compartir
        function updateShareLink() {
            const currentUrl = window.location.href;
            document.getElementById('shareLink').value = currentUrl;
        }

        // Copiar link
        function copyLink() {
            const linkInput = document.getElementById('shareLink');
            linkInput.select();
            document.execCommand('copy');
            alert('¡Link copiado al portapapeles!');
        }

        // Compartir por WhatsApp
        function shareWhatsApp() {
            const link = document.getElementById('shareLink').value;
            const message = `¡Mira nuestro menú digital! 🍕\n${restaurantData.name}\n${link}`;
            const whatsappUrl = `https://wa.me/?text=${encodeURIComponent(message)}`;
            window.open(whatsappUrl, '_blank');
        }

        // Exportar datos
        function exportData() {
            const dataStr = JSON.stringify(restaurantData, null, 2);
            const dataBlob = new Blob([dataStr], {type: 'application/json'});
            const url = URL.createObjectURL(dataBlob);
            const link = document.createElement('a');
            link.href = url;
            link.download = `${restaurantData.name.replace(/[^\w\s]/gi, '')}_menu_backup.json`;
            link.click();
        }

        // Importar datos
        function importData() {
            document.getElementById('importFile').click();
        }

        // Manejar importación
        function handleImport() {
            const file = document.getElementById('importFile').files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const importedData = JSON.parse(e.target.result);
                    if (confirm('¿Reemplazar todos los datos actuales con los importados?')) {
                        restaurantData = importedData;
                        updateDisplay();
                        saveData();
                        alert('¡Datos importados exitosamente!');
                    }
                } catch (error) {
                    alert('Error al importar: archivo inválido');
                }
            };
            reader.readAsText(file);
        }

        // Resetear datos
        function resetData() {
            if (confirm('¿Estás seguro? Se perderán todos los datos.')) {
                localStorage.removeItem('restaurantData');
                location.reload();
            }
        }

        // === FUNCIONES DEL CARRITO ===

        // Agregar al carrito
        function addToCart(name, price) {
            const existingItem = cart.find(item => item.name === name);
            if (existingItem) {
                existingItem.quantity += 1;
            } else {
                cart.push({ name, price, quantity: 1 });
            }
            updateCartDisplay();
            showAddedAnimation();
        }

        // Quitar del carrito
        function removeFromCart(name) {
            const itemIndex = cart.findIndex(item => item.name === name);
            if (itemIndex > -1) {
                if (cart[itemIndex].quantity > 1) {
                    cart[itemIndex].quantity -= 1;
                } else {
                    cart.splice(itemIndex, 1);
                }
            }
            updateCartDisplay();
        }

        // Actualizar display del carrito
        function updateCartDisplay() {
            const cartCount = document.getElementById('cartCount');
            const cartItems = document.getElementById('cartItems');
            const cartTotal = document.getElementById('cartTotal');
            const customerForm = document.getElementById('customerForm');

            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            cartCount.textContent = totalItems;

            if (cart.length === 0) {
                cartItems.innerHTML = '<div class="empty-cart">Tu carrito está vacío<br>¡Agrega algunos productos deliciosos!</div>';
                customerForm.style.display = 'none';
                cartTotal.style.display = 'none';
            } else {
                cartItems.innerHTML = cart.map(item => `
                    <div class="cart-item">
                        <div class="cart-item-info">
                            <h4>${item.name}</h4>
                            <div class="cart-item-price">${(item.price * item.quantity).toFixed(2)}</div>
                        </div>
                        <div class="quantity-controls">
                            <button class="quantity-btn" onclick="removeFromCart('${item.name}')">-</button>
                            <span>${item.quantity}</span>
                            <button class="quantity-btn" onclick="addToCart('${item.name}', ${item.price})">+</button>
                        </div>
                    </div>
                `).join('');

                const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
                cartTotal.textContent = `Total: ${total.toFixed(2)}`;
                cartTotal.style.display = 'block';
                customerForm.style.display = 'block';
            }
        }

        // Toggle carrito
        function toggleCart() {
            const cartModal = document.getElementById('cartModal');
            cartModal.style.display = cartModal.style.display === 'flex' ? 'none' : 'flex';
        }

        // Enviar por WhatsApp
        function sendWhatsApp() {
            const name = document.getElementById('customerName').value;
            const phone = document.getElementById('customerPhone').value;
            const address = document.getElementById('customerAddress').value;
            const notes = document.getElementById('customerNotes').value;
            const whatsappNumber = document.getElementById('adminWhatsappNumber').value || restaurantData.whatsappNumber;

            if (!name || !phone || !address) {
                alert('Por favor completa todos los campos obligatorios');
                return;
            }

            let message = `🍕 *NUEVO PEDIDO*\n\n`;
            message += `👤 *Cliente:* ${name}\n`;
            message += `📱 *Teléfono:* ${phone}\n`;
            message += `📍 *Dirección:* ${address}\n\n`;
            message += `🛒 *Pedido:*\n`;

            let total = 0;
            cart.forEach(item => {
                message += `• ${item.name} x${item.quantity} - ${(item.price * item.quantity).toFixed(2)}\n`;
                total += item.price * item.quantity;
            });

            message += `\n💰 *Total: ${total.toFixed(2)}*`;

            if (notes) {
                message += `\n\n📝 *Notas:* ${notes}`;
            }

            const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`;
            window.open(whatsappUrl, '_blank');
        }

        // Animación de agregado
        function showAddedAnimation() {
            const cartIcon = document.querySelector('.cart-icon');
            cartIcon.style.transform = 'scale(1.2)';
            setTimeout(() => {
                cartIcon.style.transform = 'scale(1)';
            }, 200);
        }

        // Event listeners
        document.addEventListener('DOMContentLoaded', function() {
            loadData();
            updateShareLink();
            
            // Cerrar modales al hacer click fuera
            document.getElementById('cartModal').addEventListener('click', function(e) {
                if (e.target === this) toggleCart();
            });
            
            document.getElementById('editModal').addEventListener('click', function(e) {
                if (e.target === this) closeEditModal();
            });

            // Actualizar número de WhatsApp al cambiar
            document.getElementById('adminWhatsappNumber').addEventListener('change', function() {
                restaurantData.whatsappNumber = this.value;
                saveData();
            });
        });
    </script>
</body>
</html>
