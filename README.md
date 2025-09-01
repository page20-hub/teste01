<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Roleta Mágica Premium</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
        }

        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            transition: all 0.3s ease;
        }

        .glass-effect {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 20px;
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .wheel-container {
            position: relative;
            width: 320px;
            height: 320px;
            margin: 0 auto;
            perspective: 1000px;
        }

        .wheel {
            position: relative;
            width: 100%;
            height: 100%;
            transform-style: preserve-3d;
            transition: transform 3s cubic-bezier(0.17, 0.67, 0.83, 0.67);
            border-radius: 50%;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        .wheel-stand {
            position: absolute;
            top: -25px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 20;
            width: 60px;
            height: 80px;
            background: linear-gradient(45deg, #2c3e50, #34495e);
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }

        .arrow {
            position: absolute;
            top: 65px;
            left: 50%;
            transform: translateX(-50%) rotate(0deg);
            width: 0;
            height: 0;
            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-top: 25px solid #2c3e50;
            z-index: 30;
            filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.3));
        }

        .wheel-sector {
            position: absolute;
            width: 100%;
            height: 100%;
            border-radius: 50%;
            clip-path: polygon(50% 50%, 50% 0%, 100% 0%, 100% 100%, 50% 100%);
            transform-origin: center;
        }

        .sector-blue {
            background: linear-gradient(45deg, #3498db, #2980b9);
        }

        .sector-red {
            background: linear-gradient(45deg, #e74c3c, #c0392b);
            transform: rotate(180deg);
        }

        .wheel-center {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 60px;
            height: 60px;
            background: #2c3e50;
            border-radius: 50%;
            border: 5px solid white;
            z-index: 10;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
        }

        .spinning {
            animation: rotateWheel 3s cubic-bezier(0.17, 0.67, 0.83, 0.67) forwards;
        }

        @keyframes rotateWheel {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(var(--final-rotation, 1080deg)); }
        }

        .result-pulse {
            animation: pulse 0.5s ease-in-out;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .table-scroll {
            max-height: 300px;
            overflow-y: auto;
            scrollbar-width: thin;
            scrollbar-color: #667eea #f1f1f1;
        }

        .table-scroll::-webkit-scrollbar {
            width: 8px;
        }

        .table-scroll::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 4px;
        }

        .table-scroll::-webkit-scrollbar-thumb {
            background: #667eea;
            border-radius: 4px;
        }

        .highlight {
            background: linear-gradient(90deg, transparent, rgba(102, 126, 234, 0.1), transparent) !important;
            animation: highlight 2s ease-in-out;
        }

        @keyframes highlight {
            0%, 100% { background: transparent; }
            50% { background: rgba(102, 126, 234, 0.1); }
        }
    </style>
</head>
<body class="min-h-screen flex items-center justify-center p-4 md:p-6">
    <!-- Botão Admin -->
    <button onclick="showAdminLogin()" class="fixed top-6 right-6 z-50 w-12 h-12 bg-gray-800 text-white rounded-full shadow-2xl hover:bg-gray-700 transition-all duration-300 transform hover:scale-110 flex items-center justify-center">
        <i class="fas fa-user-shield text-lg"></i>
    </button>

    <!-- Container Principal -->
    <div class="glass-effect w-full max-w-2xl p-6 md:p-8 mx-auto fade-in">
        <!-- Tela Inicial -->
        <div id="welcome-screen" class="text-center">
            <h1 class="text-4xl md:text-5xl font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent mb-6">
                Seja Bem Vindo
            </h1>
            
            <div class="space-y-6">
                <div class="relative">
                    <input type="text" id="user-id" placeholder="Digite seu ID" 
                           class="w-full px-6 py-4 text-lg border-2 border-gray-200 rounded-2xl focus:border-blue-500 focus:ring-2 focus:ring-blue-200 transition-all duration-300 shadow-sm">
                    <i class="fas fa-id-card absolute right-4 top-1/2 transform -translate-y-1/2 text-gray-400"></i>
                </div>
                
                <button onclick="verifyUser()" 
                        class="w-full bg-gradient-to-r from-blue-500 to-purple-600 text-white py-4 px-8 rounded-2xl font-semibold text-lg hover:from-blue-600 hover:to-purple-700 transform hover:scale-105 transition-all duration-300 shadow-lg">
                    <i class="fas fa-arrow-right mr-2"></i>
                    Entrar na Roleta
                </button>
                
                <p id="welcome-error" class="text-red-500 font-medium text-sm"></p>
            </div>
        </div>

        <!-- Tela da Roleta -->
        <div id="roleta-screen" class="hidden text-center">
            <h1 class="text-3xl md:text-4xl font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent mb-2">
                Roleta Mágica
            </h1>
            
            <p class="text-gray-600 mb-8">Sua sorte está prestes a ser revelada!</p>
            
            <div class="wheel-container mb-8">
                <div class="wheel-stand"></div>
                <div class="arrow" id="arrow"></div>
                <div class="wheel" id="wheel">
                    <div class="wheel-sector sector-blue"></div>
                    <div class="wheel-sector sector-red"></div>
                    <div class="wheel-center"></div>
                </div>
            </div>
            
            <button id="spin-btn" onclick="spinWheel()" 
                    class="bg-gradient-to-r from-green-500 to-green-600 text-white py-3 px-8 rounded-xl font-semibold text-lg hover:from-green-600 hover:to-green-700 transform hover:scale-105 transition-all duration-300 shadow-lg mb-6">
                <i class="fas fa-sync-alt mr-2"></i>
                Girar Roleta
            </button>
            
            <div class="result-pulse p-4 rounded-2xl bg-gray-50 border-2 border-gray-100 mb-6">
                <p id="result" class="text-xl font-semibold"></p>
            </div>
            
            <button onclick="backToWelcome()" 
                    class="bg-gray-500 text-white py-2 px-6 rounded-xl font-medium hover:bg-gray-600 transition-all duration-300">
                <i class="fas fa-arrow-left mr-2"></i>
                Voltar
            </button>
        </div>
    </div>

    <!-- Overlay e Painéis do ADM -->
    <div id="admin-overlay" class="fixed inset-0 bg-black bg-opacity-50 z-40 hidden"></div>
    
    <!-- Login ADM -->
    <div id="admin-login-panel" class="glass-effect fixed top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-50 p-8 rounded-2xl w-96 hidden">
        <div class="text-center mb-6">
            <i class="fas fa-lock text-4xl text-blue-500 mb-3"></i>
            <h2 class="text-2xl font-bold text-gray-800">Área do Administrador</h2>
            <p class="text-gray-600 mt-2">Digite a senha para continuar</p>
        </div>
        
        <div class="space-y-4">
            <div class="relative">
                <input type="password" id="admin-password" placeholder="Senha" 
                       class="w-full px-6 py-3 border-2 border-gray-200 rounded-xl focus:border-blue-500 focus:ring-2 focus:ring-blue-200">
                <i class="fas fa-key absolute right-4 top-1/2 transform -translate-y-1/2 text-gray-400"></i>
            </div>
            
            <button onclick="loginAdmin()" 
                    class="w-full bg-blue-500 text-white py-3 rounded-xl font-semibold hover:bg-blue-600 transition-all duration-300">
                Entrar
            </button>
            
            <p id="admin-login-error" class="text-red-500 text-sm font-medium"></p>
        </div>
        
        <button onclick="closeAdmin()" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600">
            <i class="fas fa-times text-xl"></i>
        </button>
    </div>

    <!-- Painel Principal do ADM -->
    <div id="admin-panel" class="glass-effect fixed top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-50 p-8 rounded-2xl w-11/12 max-w-4xl max-h-[90vh] overflow-y-auto hidden">
        <div class="text-center mb-8">
            <i class="fas fa-crown text-4xl text-yellow-500 mb-3"></i>
            <h2 class="text-3xl font-bold text-gray-800">Seja Bem Vindo ADM</h2>
        </div>

        <!-- Seção Usuários -->
        <div class="mb-8">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-xl font-semibold text-gray-800">
                    <i class="fas fa-users mr-2 text-blue-500"></i>
                    Usuários Registrados
                </h3>
                <button onclick="showNewUserForm()" 
                        class="bg-green-500 text-white px-4 py-2 rounded-xl text-sm hover:bg-green-600 transition-all duration-300">
                    <i class="fas fa-plus mr-1"></i> Novo Usuário
                </button>
            </div>

            <div class="table-scroll bg-gray-50 rounded-xl p-4 border border-gray-200">
                <table class="w-full">
                    <thead>
                        <tr class="bg-gray-100">
                            <th class="px-4 py-3 text-left text-sm font-semibold text-gray-700">ID</th>
                            <th class="px-4 py-3 text-left text-sm font-semibold text-gray-700">Usuário</th>
                            <th class="px-4 py-3 text-left text-sm font-semibold text-gray-700">Ações</th>
                        </tr>
                    </thead>
                    <tbody id="users-list" class="divide-y divide-gray-200"></tbody>
                </table>
            </div>

            <div id="new-user-form" class="mt-4 p-4 bg-blue-50 rounded-xl border border-blue-200 hidden">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">ID do usuário</label>
                        <input type="text" id="new-user-id" placeholder="ID único" class="w-full px-4 py-2 border rounded-lg">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">Nome do usuário</label>
                        <input type="text" id="new-user-name" placeholder="Nome completo" class="w-full px-4 py-2 border rounded-lg">
                    </div>
                </div>
                <div class="flex gap-2">
                    <button onclick="addNewUser()" class="bg-blue-500 text-white px-4 py-2 rounded-lg text-sm hover:bg-blue-600">
                        Salvar
                    </button>
                    <button onclick="cancelNewUser()" class="bg-gray-500 text-white px-4 py-2 rounded-lg text-sm hover:bg-gray-600">
                        Cancelar
                    </button>
                </div>
                <p id="new-user-error" class="text-red-500 text-sm mt-2"></p>
            </div>
        </div>

        <!-- Seção Histórico -->
        <div class="mb-8">
            <h3 class="text-xl font-semibold text-gray-800 mb-4">
                <i class="fas fa-history mr-2 text-purple-500"></i>
                Histórico de Giros
            </h3>
            
            <div class="table-scroll bg-gray-50 rounded-xl p-4 border border-gray-200">
                <table class="w-full">
                    <thead>
                        <tr class="bg-gray-100">
                            <th class="px-4 py-3 text-left text-sm font-semibold text-gray-700">ID</th>
                            <th class="px-4 py-3 text-left text-sm font-semibold text-gray-700">Usuário</th>
                            <th class="px-4 py-3 text-left text-sm font-semibold text-gray-700">Cor</th>
                            <th class="px-4 py-3 text-left text-sm font-semibold text-gray-700">Data</th>
                            <th class="px-4 py-3 text-left text-sm font-semibold text-gray-700">Hora</th>
                        </tr>
                    </thead>
                    <tbody id="history-list" class="divide-y divide-gray-200"></tbody>
                </table>
            </div>
        </div>

        <!-- Ações do ADM -->
        <div class="flex gap-4 justify-center">
            <button onclick="resetData()" class="bg-red-500 text-white px-6 py-3 rounded-xl font-semibold hover:bg-red-600 transition-all duration-300">
                <i class="fas fa-trash mr-2"></i> Resetar Dados
            </button>
            <button onclick="saveData()" class="bg-green-500 text-white px-6 py-3 rounded-xl font-semibold hover:bg-green-600 transition-all duration-300">
                <i class="fas fa-save mr-2"></i> Salvar Dados
            </button>
        </div>

        <button onclick="closeAdmin()" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600">
            <i class="fas fa-times text-xl"></i>
        </button>
    </div>

    <script>
        // Dados e estado
        let users = JSON.parse(localStorage.getItem('roleta_users')) || {};
        let history = JSON.parse(localStorage.getItem('roleta_history')) || [];
        let currentUser = null;
        let isSpinning = false;

        // Inicialização
        document.addEventListener('DOMContentLoaded', function() {
            if (Object.keys(users).length === 0) {
                users = {
                    'admin001': 'Administrador',
                    'user001': 'João Silva',
                    'user002': 'Maria Santos',
                    'user003': 'Pedro Costa'
                };
                localStorage.setItem('roleta_users', JSON.stringify(users));
            }
        });

        // Funções principais
        function verifyUser() {
            const userId = document.getElementById('user-id').value.trim();
            const errorElement = document.getElementById('welcome-error');
            
            if (!userId) {
                errorElement.textContent = 'Por favor, digite seu ID';
                return;
            }
            
            if (users[userId]) {
                currentUser = { id: userId, name: users[userId] };
                document.getElementById('welcome-screen').classList.add('hidden');
                document.getElementById('roleta-screen').classList.remove('hidden');
                errorElement.textContent = '';
            } else {
                errorElement.textContent = 'ID não encontrado. Contate o administrador.';
            }
        }

        function spinWheel() {
            if (isSpinning) return;
            
            const spinBtn = document.getElementById('spin-btn');
            const wheel = document.getElementById('wheel');
            const result = document.getElementById('result');
            const arrow = document.getElementById('arrow');
            
            isSpinning = true;
            spinBtn.disabled = true;
            result.textContent = '🎯 Girando...';
            result.className = 'text-xl font-semibold';
            
            // Determinar resultado antecipadamente para cálculo preciso
            const isBlue = Math.random() < 0.5;
            const targetColor = isBlue ? 'azul' : 'vermelho';
            const targetRotation = isBlue ? 1080 + 90 : 1080 + 270; // 90° para azul, 270° para vermelho
            
            // Configurar animação
            wheel.style.setProperty('--final-rotation', `${targetRotation}deg`);
            wheel.classList.add('spinning');
            
            setTimeout(() => {
                wheel.classList.remove('spinning');
                wheel.style.transform = `rotate(${targetRotation}deg)`;
                
                // Ajustar seta para apontar corretamente
                setTimeout(() => {
                    const arrowColor = isBlue ? '#3498db' : '#e74c3c';
                    arrow.style.borderTopColor = arrowColor;
                    
                    // Mostrar resultado
                    result.innerHTML = `🎊 <span class="${isBlue ? 'text-blue-600' : 'text-red-600'}">A cor selecionada foi: ${targetColor.toUpperCase()}</span> 🎉`;
                    result.classList.add('result-pulse');
                    
                    // Registrar no histórico
                    const now = new Date();
                    const spinData = {
                        id: currentUser.id,
                        user: currentUser.name,
                        color: targetColor,
                        date: now.toLocaleDateString('pt-BR'),
                        time: now.toLocaleTimeString('pt-BR', { hour: '2-digit', minute: '2-digit' })
                    };
                    
                    history.push(spinData);
                    localStorage.setItem('roleta_history', JSON.stringify(history));
                    
                    // Atualizar histórico do ADM se estiver aberto
                    if (document.getElementById('admin-panel').style.display === 'block') {
                        updateHistoryTable();
                    }
                    
                    spinBtn.disabled = false;
                    isSpinning = false;
                }, 100);
            }, 3000);
        }

        function backToWelcome() {
            document.getElementById('roleta-screen').classList.add('hidden');
            document.getElementById('welcome-screen').classList.remove('hidden');
            document.getElementById('user-id').value = '';
            document.getElementById('result').textContent = '';
            document.getElementById('result').className = 'text-xl font-semibold';
            
            // Resetar roleta
            const wheel = document.getElementById('wheel');
            const arrow = document.getElementById('arrow');
            wheel.style.transform = 'rotate(0deg)';
            arrow.style.borderTopColor = '#2c3e50';
            
            currentUser = null;
        }

        // Funções do ADM
        function showAdminLogin() {
            document.getElementById('admin-overlay').style.display = 'block';
            document.getElementById('admin-login-panel').style.display = 'block';
        }

        function loginAdmin() {
            const password = document.getElementById('admin-password').value;
            const errorElement = document.getElementById('admin-login-error');
            
            if (password === '200822') {
                document.getElementById('admin-login-panel').style.display = 'none';
                document.getElementById('admin-panel').style.display = 'block';
                updateUsersTable();
                updateHistoryTable();
                errorElement.textContent = '';
            } else {
                errorElement.textContent = 'Senha incorreta';
            }
        }

        function closeAdmin() {
            document.getElementById('admin-overlay').style.display = 'none';
            document.getElementById('admin-login-panel').style.display = 'none';
            document.getElementById('admin-panel').style.display = 'none';
            document.getElementById('admin-password').value = '';
            document.getElementById('admin-login-error').textContent = '';
        }

        function updateUsersTable() {
            const usersList = document.getElementById('users-list');
            usersList.innerHTML = '';
            
            Object.entries(users).forEach(([id, name]) => {
                const row = document.createElement('tr');
                row.className = 'hover:bg-gray-50';
                row.innerHTML = `
                    <td class="px-4 py-3 font-mono text-sm">${id}</td>
                    <td class="px-4 py-3 text-sm">${name}</td>
                    <td class="px-4 py-3">
                        <button onclick="editUser('${id}')" class="bg-blue-500 text-white px-3 py-1 rounded-lg text-xs hover:bg-blue-600 mr-2">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button onclick="deleteUser('${id}')" class="bg-red-500 text-white px-3 py-1 rounded-lg text-xs hover:bg-red-600">
                            <i class="fas fa-trash"></i>
                        </button>
                    </td>
                `;
                usersList.appendChild(row);
            });
        }

        function updateHistoryTable() {
            const historyList = document.getElementById('history-list');
            historyList.innerHTML = '';
            
            const sortedHistory = [...history].sort((a, b) => {
                const dateA = new Date(`${a.date} ${a.time}`);
                const dateB = new Date(`${b.date} ${b.time}`);
                return dateB - dateA;
            });
            
            sortedHistory.forEach((entry, index) => {
                const row = document.createElement('tr');
                row.className = 'hover:bg-gray-50';
                if (index === 0) row.className += ' highlight';
                
                row.innerHTML = `
                    <td class="px-4 py-3 font-mono text-sm">${entry.id}</td>
                    <td class="px-4 py-3 text-sm">${entry.user}</td>
                    <td class="px-4 py-3 text-sm font-semibold ${entry.color === 'azul' ? 'text-blue-600' : 'text-red-600'}">
                        ${entry.color.toUpperCase()}
                    </td>
                    <td class="px-4 py-3 text-sm">${entry.date}</td>
                    <td class="px-4 py-3 text-sm">${entry.time}</td>
                `;
                historyList.appendChild(row);
            });
        }

        function showNewUserForm() {
            document.getElementById('new-user-form').classList.remove('hidden');
        }

        function cancelNewUser() {
            document.getElementById('new-user-form').classList.add('hidden');
            document.getElementById('new-user-id').value = '';
            document.getElementById('new-user-name').value = '';
            document.getElementById('new-user-error').textContent = '';
        }

        function addNewUser() {
            const id = document.getElementById('new-user-id').value.trim();
            const name = document.getElementById('new-user-name').value.trim();
            const errorElement = document.getElementById('new-user-error');
            
            if (!id || !name) {
                errorElement.textContent = 'Preencha todos os campos';
                return;
            }
            
            if (users[id]) {
                errorElement.textContent = 'ID já existe';
                return;
            }
            
            users[id] = name;
            localStorage.setItem('roleta_users', JSON.stringify(users));
            updateUsersTable();
            cancelNewUser();
            
            // Destacar nova entrada
            const newRow = Array.from(document.querySelectorAll('#users-list tr')).find(tr => 
                tr.querySelector('td:first-child').textContent === id
            );
            if (newRow) newRow.classList.add('highlight');
        }

        function editUser(id) {
            const newName = prompt('Novo nome para o usuário:', users[id]);
            if (newName && newName.trim()) {
                users[id] = newName.trim();
                localStorage.setItem('roleta_users', JSON.stringify(users));
                updateUsersTable();
            }
        }

        function deleteUser(id) {
            if (confirm(`Tem certeza que deseja excluir o usuário "${users[id]}"?`)) {
                delete users[id];
                localStorage.setItem('roleta_users', JSON.stringify(users));
                
                history = history.filter(entry => entry.id !== id);
                localStorage.setItem('roleta_history', JSON.stringify(history));
                
                updateUsersTable();
                updateHistoryTable();
            }
        }

        function resetData() {
            if (confirm('Tem certeza que deseja resetar TODOS os dados? Isso apagará todos os usuários e o histórico.')) {
                users = {
                    'admin001': 'Administrador',
                    'user001': 'Usuário Exemplo 1'
                };
                history = [];
                localStorage.setItem('roleta_users', JSON.stringify(users));
                localStorage.setItem('roleta_history', JSON.stringify(history));
                updateUsersTable();
                updateHistoryTable();
            }
        }

        function saveData() {
            localStorage.setItem('roleta_users', JSON.stringify(users));
            localStorage.setItem('roleta_history', JSON.stringify(history));
            
            // Feedback visual
            const saveBtn = document.querySelector('#admin-panel button:last-child');
            const originalText = saveBtn.innerHTML;
            saveBtn.innerHTML = '<i class="fas fa-check mr-2"></i>Salvo!';
            saveBtn.classList.add('bg-green-600');
            
            setTimeout(() => {
                saveBtn.innerHTML = originalText;
                saveBtn.classList.remove('bg-green-600');
            }, 2000);
        }
    </script>
</body>
</html>
