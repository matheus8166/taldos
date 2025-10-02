<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Gerador de Orçamentos</title>
    
    <!-- Melhoras para iPhone (Web App) -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Orçamentos">
    <link rel="apple-touch-icon" href="https://placehold.co/180x180/2563eb/ffffff?text=Orc">

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        /* Estilo para a barra de rolagem */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1f2937;
        }
        ::-webkit-scrollbar-thumb {
            background: #4b5563;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #6b7280;
        }
    </style>
</head>
<body class="bg-gray-900 text-white">

    <div class="container mx-auto p-4 md:p-8 pb-24 lg:pb-8">
        <header class="text-center mb-8">
            <h1 class="text-4xl font-bold text-blue-400">Gerador de Orçamentos</h1>
            <p class="text-gray-400 mt-2">Crie e envie orçamentos de forma rápida e eficiente.</p>
        </header>

        <main class="lg:grid lg:grid-cols-2 lg:gap-8">

            <!-- Coluna da Esquerda: Produtos e Configurações (Visível por padrão no mobile) -->
            <div id="products-view" class="bg-gray-800 p-6 rounded-lg shadow-lg flex flex-col">
                <h2 class="text-2xl font-semibold mb-4 border-b border-gray-700 pb-2">1. Configurações e Produtos</h2>

                <!-- Seção de Configurações -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-6">
                    <div>
                        <label for="customerName" class="block text-sm font-medium text-gray-300 mb-1">Nome do Cliente:</label>
                        <input type="text" id="customerName" class="w-full bg-gray-700 border border-gray-600 rounded-lg p-2 focus:ring-blue-500 focus:border-blue-500" placeholder="João da Silva">
                    </div>
                    <div>
                        <label for="customerCity" class="block text-sm font-medium text-gray-300 mb-1">Cidade do Cliente:</label>
                        <input type="text" id="customerCity" class="w-full bg-gray-700 border border-gray-600 rounded-lg p-2 focus:ring-blue-500 focus:border-blue-500" placeholder="São Paulo">
                    </div>
                    <div class="md:col-span-2">
                        <label for="pesoRate" class="block text-sm font-medium text-gray-300 mb-1">Cotação do Peso Argentino (ARS):</label>
                        <input type="number" id="pesoRate" step="0.01" class="w-full bg-gray-700 border border-gray-600 rounded-lg p-2 focus:ring-blue-500 focus:border-blue-500" placeholder="Ex: 5.50">
                    </div>
                </div>

                <!-- Seção de Busca de Produtos -->
                <div class="flex-grow flex flex-col">
                    <label for="searchProduct" class="block text-sm font-medium text-gray-300 mb-1">Pesquisar Produto:</label>
                    <input type="text" id="searchProduct" class="w-full bg-gray-700 border border-gray-600 rounded-lg p-2 mb-4 focus:ring-blue-500 focus:border-blue-500" placeholder="Digite a referência ou nome do produto">

                    <div id="productList" class="flex-grow overflow-y-auto bg-gray-900/50 rounded-lg p-2 h-96">
                        <!-- Produtos serão inseridos aqui via JavaScript -->
                    </div>
                </div>
            </div>

            <!-- Coluna da Direita: Orçamento Atual (Oculta por padrão no mobile) -->
            <div id="budget-view" class="bg-gray-800 p-6 rounded-lg shadow-lg hidden lg:flex lg:flex-col">
                <h2 class="text-2xl font-semibold mb-4 border-b border-gray-700 pb-2">2. Orçamento Atual</h2>
                
                <div class="overflow-x-auto h-[35rem] mb-4">
                    <table class="min-w-full">
                        <thead class="bg-gray-700 sticky top-0">
                            <tr>
                                <th class="text-left p-3">Produto</th>
                                <th class="text-center p-3">Qtd. (Pares)</th>
                                <th class="text-right p-3">Subtotal</th>
                                <th class="text-center p-3">Ação</th>
                            </tr>
                        </thead>
                        <tbody id="budgetItems">
                           <!-- Itens do orçamento serão inseridos aqui -->
                           <tr id="empty-message">
                               <td colspan="4" class="text-center text-gray-400 p-8">Nenhum produto adicionado ainda.</td>
                           </tr>
                        </tbody>
                    </table>
                </div>

                <!-- Seção de Resumo -->
                <div class="bg-gray-900/70 p-4 rounded-lg space-y-3">
                    <h3 class="text-xl font-semibold mb-2">Resumo do Pedido</h3>
                    <div class="flex justify-between items-center text-lg">
                        <span class="text-gray-300">Subtotal Produtos:</span>
                        <span id="subtotalProducts" class="font-bold text-blue-300">R$ 0,00</span>
                    </div>
                     <div class="flex justify-between items-center text-sm">
                        <span class="text-gray-400">Custo Nota Fiscal (R$ 1,00/par):</span>
                        <span id="costNota" class="font-semibold text-gray-400">R$ 0,00</span>
                    </div>
                    <div class="flex justify-between items-center text-sm">
                        <span class="text-gray-400">Custo Frete (R$ 3,00/par):</span>
                        <span id="costFrete" class="font-semibold text-gray-400">R$ 0,00</span>
                    </div>
                    <hr class="border-gray-600 my-2">
                    <div class="flex justify-between items-center text-2xl">
                        <span class="text-white font-bold">TOTAL (BRL):</span>
                        <span id="grandTotalBRL" class="font-bold text-green-400">R$ 0,00</span>
                    </div>
                    <div class="flex justify-between items-center text-xl">
                        <span class="text-gray-300">TOTAL (ARS):</span>
                        <span id="grandTotalARS" class="font-semibold text-yellow-400">$ 0,00</span>
                    </div>
                </div>

                <button id="sendWhatsApp" class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-4 rounded-lg mt-6 transition duration-300 flex items-center justify-center gap-2">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path><path d="M14.05 2a9 9 0 0 1 8 7.94"></path><path d="M14.05 6A5 5 0 0 1 18 10"></path></svg>
                    Enviar Orçamento via WhatsApp
                </button>
            </div>
        </main>
    </div>

    <!-- Mobile Navigation -->
    <nav class="lg:hidden fixed bottom-0 left-0 right-0 bg-gray-800 border-t border-gray-700 p-2 flex justify-around z-50 shadow-lg">
        <button id="show-products-btn" class="flex flex-col items-center justify-center text-center text-gray-300 font-semibold p-2 w-full rounded-md transition-colors duration-200">
            <svg class="w-6 h-6 mb-1" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 10h16M4 14h16M4 18h16"></path></svg>
            <span>Produtos</span>
        </button>
        <button id="show-budget-btn" class="flex flex-col items-center justify-center text-center text-gray-300 font-semibold p-2 w-full rounded-md relative transition-colors duration-200">
             <svg class="w-6 h-6 mb-1" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01"></path></svg>
            <span>Orçamento</span>
            <span id="budget-count-badge" class="absolute top-1 right-5 bg-red-500 text-white text-xs rounded-full h-5 w-5 flex items-center justify-center hidden transform translate-x-1/2 -translate-y-1/2">0</span>
        </button>
    </nav>

    <script>
        // Dados da planilha CSV fornecida pelo usuário
        const csvData = `REFERENCIA,PRODUTO,VALOR
PM3002,PUMA 180 REVERSE,90
MA35,JORDAN LOW,73
KN521,NIKE ZOOM VOMERO 5,90
N1906A,NEW BALANCE 1906,90
N2002,NEW BALANCE 2002,88
ONRNG,ONE RUNNING,90
TMR101,TOMMY HILFIGER,73
JRD1100,JORDAN TRAVIS SCOTT,90
ADSL,ADIDAS SL72,88
ADSLN,ADIDAS SL 72 ,88
DNSUB,NIKE DN,88
LC001,LACOSTE,88
AD001,ADIDAS OZMILLEN,90
AD20900,ADIDAS CURT + MEIA,75
F8262,PUMA PALERMO,73
F19031,PUMA CAVEN,73
ADSDROP,ADIDAS DROP STEP,100
NB1080A,NEW BALANCE 1080,100
FA134,ADIDAS SPACE,73
F7411,ADIDAS,73
NB327,NEW BALANCE 327,80
F2512,NEW BALANCE 458,78
AD100/AD101,ADIDAS SAMBA XLG,78
CAD2501,ADIDAS AD2000,100
KN08,AIRMAX,85
IMAMR01,AMIRI,190
RNH1110,DUNK SB,75
IMLAC01,LACOSTE NEO,190
LC2001,LACOSTE NEW ZONE,78
IMNEW01,NEW BALANCE 1000,190
N1000A,NEW BALANCE 1000,90
IMNEW02,NEW BALANCE 2000,230
NBR301,NEW BALANCE 574,78
GNEW01,NEW BALANCE 9060,95
PD2055,PRADA MILANO,85
PR1000,PRADA MILANO,80
PRD001,PRADA MILANO,95
IMPDR01,THUNDER PRADA,190
KEDSAM01,SAMBA,75
VN2026,VANS KNUL,68
PM180,PUMA 180,90
PM3003,PUMA 180,90
NGJOR01,JORDAN LOW,80
NGDUN01,DUNK,100
NGCAMP,CAMPUS,90
NGAIR01,AIRFORCE TR,85
0126AS,ADIDAS SUPERSTAR,70
NGINVEN01,NIKE INVECIBLE,66
NGCONVER01,CONVERSE RUN,63
KEDZOOM1,NIKE ZOOM X,66
NB570,NEW BALANCE 550,100
KEDVOM01,NIKE VOMERO,66`;

        // Estado da aplicação
        let products = [];
        let budgetItems = [];

        // Elementos do DOM
        const productListEl = document.getElementById('productList');
        const searchInput = document.getElementById('searchProduct');
        const budgetItemsEl = document.getElementById('budgetItems');
        const pesoRateInput = document.getElementById('pesoRate');
        // Mobile-specific elements
        const productsView = document.getElementById('products-view');
        const budgetView = document.getElementById('budget-view');
        const showProductsBtn = document.getElementById('show-products-btn');
        const showBudgetBtn = document.getElementById('show-budget-btn');
        const budgetCountBadge = document.getElementById('budget-count-badge');

        // Função para processar o CSV
        function parseCSV(data) {
            const lines = data.trim().split('\n');
            const header = lines.shift().split(',');
            return lines.map(line => {
                const values = line.split(',');
                return {
                    referencia: values[0].trim(),
                    produto: values[1].trim(),
                    valor: parseFloat(values[2])
                };
            });
        }

        // Função para renderizar a lista de produtos
        function renderProducts(filteredProducts) {
            productListEl.innerHTML = '';
            if (filteredProducts.length === 0) {
                productListEl.innerHTML = '<p class="text-center text-gray-500 p-4">Nenhum produto encontrado.</p>';
                return;
            }
            filteredProducts.forEach(product => {
                const productEl = document.createElement('div');
                productEl.className = 'flex justify-between items-center p-3 hover:bg-gray-700 rounded-lg transition-colors duration-200';
                productEl.innerHTML = `
                    <div>
                        <p class="font-semibold">${product.produto} (${product.referencia})</p>
                        <p class="text-sm text-blue-400">R$ ${product.valor.toFixed(2)}</p>
                    </div>
                    <button data-ref="${product.referencia}" class="add-btn bg-blue-600 hover:bg-blue-700 text-white font-bold py-1 px-3 rounded-lg transition duration-300">+</button>
                `;
                productListEl.appendChild(productEl);
            });
        }
        
        // Função para atualizar o resumo e os totais
        function updateSummary() {
            let subtotalProducts = 0;
            let totalPairs = 0;

            budgetItems.forEach(item => {
                subtotalProducts += item.valor * item.quantity;
                totalPairs += item.quantity;
            });
            
            const costNota = totalPairs * 1.00;
            const costFrete = totalPairs * 3.00;
            const grandTotalBRL = subtotalProducts + costNota + costFrete;
            
            const pesoRate = parseFloat(pesoRateInput.value) || 0;
            const grandTotalARS = grandTotalBRL * pesoRate;

            document.getElementById('subtotalProducts').textContent = `R$ ${subtotalProducts.toFixed(2)}`;
            document.getElementById('costNota').textContent = `R$ ${costNota.toFixed(2)}`;
            document.getElementById('costFrete').textContent = `R$ ${costFrete.toFixed(2)}`;
            document.getElementById('grandTotalBRL').textContent = `R$ ${grandTotalBRL.toFixed(2)}`;
            document.getElementById('grandTotalARS').textContent = `$ ${grandTotalARS.toFixed(2)}`;
        }
        
        function updateBadge() {
            const count = budgetItems.length;
            if (count > 0) {
                budgetCountBadge.textContent = count;
                budgetCountBadge.classList.remove('hidden');
            } else {
                budgetCountBadge.classList.add('hidden');
            }
        }

        // Função para renderizar os itens no orçamento
        function renderBudget() {
            budgetItemsEl.innerHTML = '';
            const emptyMessage = document.getElementById('empty-message');

            if (budgetItems.length === 0) {
                 if(emptyMessage) emptyMessage.style.display = 'table-row';
            } else {
                if(emptyMessage) emptyMessage.style.display = 'none';
                budgetItems.forEach(item => {
                    const itemEl = document.createElement('tr');
                    itemEl.className = 'border-b border-gray-700';
                    itemEl.innerHTML = `
                        <td class="p-3">
                            <p class="font-semibold">${item.produto}</p>
                            <p class="text-xs text-gray-400">${item.referencia}</p>
                        </td>
                        <td class="p-3 text-center">${item.quantity}</td>
                        <td class="p-3 text-right font-mono">R$ ${(item.valor * item.quantity).toFixed(2)}</td>
                        <td class="p-3 text-center">
                            <button data-id="${item.id}" class="remove-btn bg-red-600 hover:bg-red-700 text-white font-bold py-1 px-2 rounded-lg text-xs">X</button>
                        </td>
                    `;
                    budgetItemsEl.appendChild(itemEl);
                });
            }
            updateSummary();
            updateBadge();
        }

        // Função para adicionar item ao orçamento
        function addToBudget(ref) {
            const product = products.find(p => p.referencia === ref);
            if (product) {
                // Adiciona um ID único para cada item, permitindo duplicatas
                const uniqueId = crypto.randomUUID();
                budgetItems.push({ ...product, quantity: 12, id: uniqueId }); 
                renderBudget();
            }
        }
        
        // Função para remover item do orçamento pelo seu ID único
        function removeFromBudget(id) {
            budgetItems = budgetItems.filter(item => item.id !== id);
            renderBudget();
        }
        
        // Função para enviar o orçamento via WhatsApp
        function sendWhatsAppMessage() {
            const customerName = document.getElementById('customerName').value.trim();
            const customerCity = document.getElementById('customerCity').value.trim();
            
            if (!customerName || !customerCity) {
                alert('Por favor, preencha o nome e a cidade do cliente.');
                return;
            }
            if (budgetItems.length === 0) {
                alert('Adicione pelo menos um produto ao orçamento.');
                return;
            }

            let message = `*>> NOVO ORÇAMENTO <<*\n\n`;
            message += `*Cliente:* ${customerName}\n`;
            message += `*Cidade:* ${customerCity}\n`;
            message += `--------------------------------------\n\n`;
            message += `*ITENS DO PEDIDO:*\n\n`;

            let totalPairs = 0;
            budgetItems.forEach(item => {
                message += `*Produto:* ${item.produto} (${item.referencia})\n`;
                message += `*Qtd:* ${item.quantity} pares\n`;
                message += `*Subtotal:* R$ ${(item.valor * item.quantity).toFixed(2)}\n\n`;
                totalPairs += item.quantity;
            });
            
            const subtotalProducts = budgetItems.reduce((acc, item) => acc + (item.valor * item.quantity), 0);
            const costNota = totalPairs * 1.00;
            const costFrete = totalPairs * 3.00;
            const grandTotalBRL = subtotalProducts + costNota + costFrete;
            const pesoRate = parseFloat(pesoRateInput.value) || 0;
            const grandTotalARS = grandTotalBRL * pesoRate;

            message += `--------------------------------------\n`;
            message += `*RESUMO GERAL:*\n`;
            message += `*Total de Pares:* ${totalPairs}\n`;
            message += `*Subtotal Produtos:* R$ ${subtotalProducts.toFixed(2)}\n`;
            message += `*Nota Fiscal:* R$ ${costNota.toFixed(2)}\n`;
            message += `*Frete:* R$ ${costFrete.toFixed(2)}\n\n`;
            message += `*VALOR TOTAL (BRL): R$ ${grandTotalBRL.toFixed(2)}*\n`;
            if (pesoRate > 0) {
                 message += `*VALOR TOTAL (ARS): $ ${grandTotalARS.toFixed(2)}*\n`;
                 message += `_(Cotação: ${pesoRate.toFixed(2)})_\n`;
            }
            
            const phoneNumber = '5537998457276';
            const encodedMessage = encodeURIComponent(message);
            const whatsappUrl = `https://wa.me/${phoneNumber}?text=${encodedMessage}`;
            
            window.open(whatsappUrl, '_blank');
        }

        // Event Listeners
        document.addEventListener('DOMContentLoaded', () => {
            products = parseCSV(csvData);
            renderProducts(products);
            // Set initial active button state for mobile
            showProductsBtn.classList.add('bg-gray-700', 'text-blue-400');
            showProductsBtn.classList.remove('text-gray-300');
        });

        searchInput.addEventListener('input', (e) => {
            const searchTerm = e.target.value.toLowerCase();
            const filtered = products.filter(p => 
                p.produto.toLowerCase().includes(searchTerm) || 
                p.referencia.toLowerCase().includes(searchTerm)
            );
            renderProducts(filtered);
        });

        productListEl.addEventListener('click', (e) => {
            if (e.target.classList.contains('add-btn')) {
                const ref = e.target.dataset.ref;
                addToBudget(ref);
            }
        });
        
        budgetItemsEl.addEventListener('click', (e) => {
            if (e.target.classList.contains('remove-btn')) {
                const id = e.target.dataset.id; // Pega o ID único
                removeFromBudget(id); // Remove pelo ID
            }
        });
        
        pesoRateInput.addEventListener('input', updateSummary);
        
        document.getElementById('sendWhatsApp').addEventListener('click', sendWhatsAppMessage);

        // Mobile navigation logic
        showProductsBtn.addEventListener('click', () => {
            productsView.classList.remove('hidden');
            budgetView.classList.add('hidden');
            // Active state styles
            showProductsBtn.classList.add('bg-gray-700', 'text-blue-400');
            showProductsBtn.classList.remove('text-gray-300');
            showBudgetBtn.classList.remove('bg-gray-700', 'text-blue-400');
            showBudgetBtn.classList.add('text-gray-300');
        });

        showBudgetBtn.addEventListener('click', () => {
            budgetView.classList.remove('hidden');
            productsView.classList.add('hidden');
            // Active state styles
            showBudgetBtn.classList.add('bg-gray-700', 'text-blue-400');
            showBudgetBtn.classList.remove('text-gray-300');
            showProductsBtn.classList.remove('bg-gray-700', 'text-blue-400');
            showProductsBtn.classList.add('text-gray-300');
        });

    </script>
</body>
</html>
