<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GamificaGestão</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .gradient-bg { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }
        .game-gradient { background: linear-gradient(135deg, #ff6b6b 0%, #4ecdc4 50%, #45b7d1 100%); }
        .card-hover { transition: all 0.3s ease; }
        .card-hover:hover { transform: translateY(-5px) scale(1.02); box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.2); }
        .sidebar-transition { transition: transform 0.3s ease; }
        .task-card {
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        .task-card:hover {
            transform: scale(1.05) rotate(1deg);
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }
        .task-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
            transition: left 0.5s;
        }
        .task-card:hover::before {
            left: 100%;
        }
        .progress-bar {
            transition: width 0.8s ease;
            background: linear-gradient(90deg, #ff6b6b, #4ecdc4, #45b7d1);
        }
        .xp-bar {
            background: linear-gradient(90deg, #ffd700, #ffed4e);
            transition: width 0.5s ease;
        }
        .level-badge {
            background: linear-gradient(135deg, #ffd700, #ffb347);
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-10px); }
            60% { transform: translateY(-5px); }
        }
        .bounce { animation: bounce 1s; }
        @keyframes sparkle {
            0%, 100% { opacity: 0; transform: scale(0); }
            50% { opacity: 1; transform: scale(1); }
        }
        .sparkle {
            animation: sparkle 1.5s infinite;
        }
        .achievement-popup {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1000;
            transform: translateX(400px);
            transition: transform 0.5s ease;
        }
        .achievement-popup.show {
            transform: translateX(0);
        }
        .coin-animation {
            animation: bounce 0.6s ease;
        }
        .streak-fire {
            animation: flicker 1s infinite alternate;
        }
        @keyframes flicker {
            0% { opacity: 1; }
            100% { opacity: 0.7; }
        }
    </style>
</head>
<body class="min-h-screen bg-gray-50">
    <!-- Tela de Login -->
    <div id="loginScreen" class="min-h-screen gradient-bg flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-md">
            <div class="text-center mb-8">
                <h1 class="text-3xl font-bold text-gray-800 mb-2">🎮 GamificaGestão</h1>
                <p class="text-gray-600">Faça login para continuar</p>
            </div>
           
            <form id="loginForm" class="space-y-6">
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-2">ID do Departamento</label>
                    <input type="text" id="departmentId" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent" placeholder="Digite seu ID" required>
                </div>
               
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-2">Senha</label>
                    <input type="password" id="password" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent" placeholder="Digite sua senha" required>
                </div>
               
                <button type="submit" class="w-full bg-blue-600 text-white py-3 rounded-lg font-medium hover:bg-blue-700 transition duration-200">
                    Entrar
                </button>
               
                <button type="button" id="forgotPassword" class="w-full text-blue-600 hover:text-blue-800 transition duration-200">
                    Esqueceu sua senha?
                </button>
            </form>
        </div>
    </div>

    <!-- Tela de Recuperação de Senha -->
    <div id="forgotPasswordScreen" class="min-h-screen gradient-bg flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-md">
            <div class="text-center mb-8">
                <h1 class="text-3xl font-bold text-gray-800 mb-2">🔐 Recuperar Senha</h1>
                <p class="text-gray-600">Digite seu e-mail para recuperação</p>
            </div>
           
            <form id="recoveryForm" class="space-y-6">
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-2">E-mail</label>
                    <input type="email" id="recoveryEmail" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent" placeholder="Digite seu e-mail" required>
                </div>
               
                <button type="submit" class="w-full bg-green-600 text-white py-3 rounded-lg font-medium hover:bg-green-700 transition duration-200">
                    Enviar Link de Recuperação
                </button>
               
                <button type="button" id="backToLogin" class="w-full text-blue-600 hover:text-blue-800 transition duration-200">
                    Voltar ao Login
                </button>
            </form>
        </div>
    </div>

    <!-- Tela de Seleção de Departamentos -->
    <div id="departmentScreen" class="min-h-screen bg-gray-50 hidden">
        <div class="container mx-auto px-4 py-8">
            <div class="text-center mb-8">
                <h1 class="text-4xl font-bold text-gray-800 mb-4">🎮 GamificaGestão</h1>
                <p class="text-xl text-gray-600">Selecione seu departamento</p>
            </div>
           
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 max-w-6xl mx-auto">
                <div class="department-card bg-white rounded-xl shadow-lg p-6 cursor-pointer card-hover" data-dept="rh">
                    <div class="text-center">
                        <div class="text-6xl mb-4">👥</div>
                        <h3 class="text-xl font-semibold text-gray-800">Recursos Humanos</h3>
                        <p class="text-gray-600 mt-2">Gestão de pessoas e talentos</p>
                    </div>
                </div>
               
                <div class="department-card bg-white rounded-xl shadow-lg p-6 cursor-pointer card-hover" data-dept="ti">
                    <div class="text-center">
                        <div class="text-6xl mb-4">💻</div>
                        <h3 class="text-xl font-semibold text-gray-800">Tecnologia da Informação</h3>
                        <p class="text-gray-600 mt-2">Desenvolvimento e infraestrutura</p>
                    </div>
                </div>
               
                <div class="department-card bg-white rounded-xl shadow-lg p-6 cursor-pointer card-hover" data-dept="marketing">
                    <div class="text-center">
                        <div class="text-6xl mb-4">📈</div>
                        <h3 class="text-xl font-semibold text-gray-800">Marketing</h3>
                        <p class="text-gray-600 mt-2">Estratégias e campanhas</p>
                    </div>
                </div>
               
                <div class="department-card bg-white rounded-xl shadow-lg p-6 cursor-pointer card-hover" data-dept="vendas">
                    <div class="text-center">
                        <div class="text-6xl mb-4">💰</div>
                        <h3 class="text-xl font-semibold text-gray-800">Vendas</h3>
                        <p class="text-gray-600 mt-2">Relacionamento com clientes</p>
                    </div>
                </div>
               
                <div class="department-card bg-white rounded-xl shadow-lg p-6 cursor-pointer card-hover" data-dept="financeiro">
                    <div class="text-center">
                        <div class="text-6xl mb-4">📊</div>
                        <h3 class="text-xl font-semibold text-gray-800">Financeiro</h3>
                        <p class="text-gray-600 mt-2">Controle financeiro e contábil</p>
                    </div>
                </div>
               
                <div class="department-card bg-white rounded-xl shadow-lg p-6 cursor-pointer card-hover" data-dept="operacoes">
                    <div class="text-center">
                        <div class="text-6xl mb-4">⚙️</div>
                        <h3 class="text-xl font-semibold text-gray-800">Operações</h3>
                        <p class="text-gray-600 mt-2">Processos e logística</p>
                    </div>
                </div>
            </div>
           
            <div class="text-center mt-8">
                <button id="logoutFromDept" class="bg-red-500 text-white px-6 py-3 rounded-lg font-medium hover:bg-red-600 transition duration-200">
                    🚪 Sair
                </button>
            </div>
        </div>
    </div>

    <!-- Tela Principal de Gestão -->
    <div id="mainScreen" class="min-h-screen bg-gray-50 hidden">
        <!-- Sidebar Gamificada -->
        <div id="sidebar" class="fixed left-0 top-0 h-full w-64 bg-gradient-to-b from-purple-900 via-blue-900 to-indigo-900 shadow-2xl z-50 sidebar-transition border-r-4 border-yellow-400">
            <!-- Header do Painel com Avatar -->
            <div class="p-6 border-b border-purple-700 bg-gradient-to-r from-purple-800 to-blue-800">
                <div class="text-center mb-4">
                    <div class="w-16 h-16 mx-auto mb-3 bg-gradient-to-br from-yellow-400 to-orange-500 rounded-full flex items-center justify-center text-2xl shadow-lg animate-pulse">
                        🎮
                    </div>
                    <h2 class="text-lg font-bold text-white">Painel de Comando</h2>
                    <div class="text-xs text-purple-200">Centro de Controle Épico</div>
                </div>
               
                <!-- Mini Stats no Header -->
                <div class="grid grid-cols-2 gap-2 text-xs">
                    <div class="bg-white bg-opacity-20 rounded-lg p-2 text-center">
                        <div class="text-yellow-300 font-bold" id="sidebarLevel">Nv.${gameData.level}</div>
                        <div class="text-purple-200">Nível</div>
                    </div>
                    <div class="bg-white bg-opacity-20 rounded-lg p-2 text-center">
                        <div class="text-green-300 font-bold" id="sidebarCoins">${gameData.coins}</div>
                        <div class="text-purple-200">Moedas</div>
                    </div>
                </div>
            </div>
           
            <!-- Menu Gamificado -->
            <nav class="p-4 space-y-3 overflow-y-auto h-full pb-32">
                <!-- Relatórios - Missão Principal -->
                <button class="sidebar-btn w-full text-left px-4 py-4 rounded-xl bg-gradient-to-r from-blue-600 to-cyan-600 hover:from-blue-500 hover:to-cyan-500 text-white transition-all duration-300 transform hover:scale-105 hover:shadow-xl border-2 border-cyan-400 relative overflow-hidden group" data-action="reports">
                    <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white to-transparent opacity-0 group-hover:opacity-20 transform -skew-x-12 group-hover:translate-x-full transition-all duration-700"></div>
                    <div class="flex items-center space-x-3 relative z-10">
                        <span class="text-2xl animate-bounce">📊</span>
                        <div>
                            <div class="font-bold">Relatórios Mensais</div>
                            <div class="text-xs text-cyan-200">Missão Principal</div>
                        </div>
                    </div>
                    <div class="absolute top-1 right-1 w-3 h-3 bg-yellow-400 rounded-full animate-ping"></div>
                </button>

                <!-- Ranking de Funcionários -->
                <button class="sidebar-btn w-full text-left px-4 py-4 rounded-xl bg-gradient-to-r from-yellow-600 to-orange-600 hover:from-yellow-500 hover:to-orange-500 text-white transition-all duration-300 transform hover:scale-105 hover:shadow-xl border-2 border-orange-400 relative overflow-hidden group" data-action="topEmployees">
                    <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white to-transparent opacity-0 group-hover:opacity-20 transform -skew-x-12 group-hover:translate-x-full transition-all duration-700"></div>
                    <div class="flex items-center space-x-3 relative z-10">
                        <span class="text-2xl">🏆</span>
                        <div>
                            <div class="font-bold">Hall da Fama</div>
                            <div class="text-xs text-orange-200">Top Jogadores</div>
                        </div>
                    </div>
                    <div class="absolute -top-1 -right-1 text-xs bg-red-500 text-white px-2 py-1 rounded-full font-bold animate-pulse">HOT</div>
                </button>

                <!-- Ranking de Departamentos -->
                <button class="sidebar-btn w-full text-left px-4 py-4 rounded-xl bg-gradient-to-r from-green-600 to-emerald-600 hover:from-green-500 hover:to-emerald-500 text-white transition-all duration-300 transform hover:scale-105 hover:shadow-xl border-2 border-emerald-400 relative overflow-hidden group" data-action="topDepartments">
                    <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white to-transparent opacity-0 group-hover:opacity-20 transform -skew-x-12 group-hover:translate-x-full transition-all duration-700"></div>
                    <div class="flex items-center space-x-3 relative z-10">
                        <span class="text-2xl">🥇</span>
                        <div>
                            <div class="font-bold">Liga dos Campeões</div>
                            <div class="text-xs text-emerald-200">Ranking Geral</div>
                        </div>
                    </div>
                </button>

                <!-- Gestão de Tarefas - Arsenal -->
                <button class="sidebar-btn w-full text-left px-4 py-4 rounded-xl bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-500 hover:to-pink-500 text-white transition-all duration-300 transform hover:scale-105 hover:shadow-xl border-2 border-pink-400 relative overflow-hidden group" data-action="taskManagement">
                    <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white to-transparent opacity-0 group-hover:opacity-20 transform -skew-x-12 group-hover:translate-x-full transition-all duration-700"></div>
                    <div class="flex items-center space-x-3 relative z-10">
                        <span class="text-2xl">⚔️</span>
                        <div>
                            <div class="font-bold">Arsenal de Missões</div>
                            <div class="text-xs text-pink-200">Centro de Comando</div>
                        </div>
                    </div>
                    <div class="absolute top-1 right-1 w-2 h-2 bg-green-400 rounded-full animate-pulse"></div>
                </button>

                <!-- Usuários Ativos - Guilda -->
                <button class="sidebar-btn w-full text-left px-4 py-4 rounded-xl bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-500 hover:to-purple-500 text-white transition-all duration-300 transform hover:scale-105 hover:shadow-xl border-2 border-purple-400 relative overflow-hidden group" data-action="activeUsers">
                    <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white to-transparent opacity-0 group-hover:opacity-20 transform -skew-x-12 group-hover:translate-x-full transition-all duration-700"></div>
                    <div class="flex items-center space-x-3 relative z-10">
                        <span class="text-2xl">👥</span>
                        <div>
                            <div class="font-bold">Membros da Guilda</div>
                            <div class="text-xs text-purple-200">Jogadores Online</div>
                        </div>
                    </div>
                    <div class="absolute -top-1 -right-1 text-xs bg-green-500 text-white px-2 py-1 rounded-full font-bold">
                        <span id="onlineCount">${users.length}</span>
                    </div>
                </button>

                <!-- Perfil - Personagem -->
                <button class="sidebar-btn w-full text-left px-4 py-4 rounded-xl bg-gradient-to-r from-gray-700 to-gray-800 hover:from-gray-600 hover:to-gray-700 text-white transition-all duration-300 transform hover:scale-105 hover:shadow-xl border-2 border-gray-500 relative overflow-hidden group" data-action="profile">
                    <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white to-transparent opacity-0 group-hover:opacity-20 transform -skew-x-12 group-hover:translate-x-full transition-all duration-700"></div>
                    <div class="flex items-center space-x-3 relative z-10">
                        <span class="text-2xl">⚙️</span>
                        <div>
                            <div class="font-bold">Configurações</div>
                            <div class="text-xs text-gray-300">Personalizar</div>
                        </div>
                    </div>
                </button>

                <!-- Seção de Conquistas Rápidas -->
                <div class="mt-6 pt-4 border-t border-purple-700">
                    <div class="text-xs text-purple-300 font-bold mb-3 text-center">🏅 CONQUISTAS RÁPIDAS</div>
                    <div class="grid grid-cols-2 gap-2">
                        <div class="bg-gradient-to-br from-yellow-500 to-orange-500 p-2 rounded-lg text-center cursor-pointer hover:scale-105 transition-transform" onclick="quickAchievement('daily')">
                            <div class="text-lg">📅</div>
                            <div class="text-xs text-white font-bold">Diária</div>
                        </div>
                        <div class="bg-gradient-to-br from-blue-500 to-cyan-500 p-2 rounded-lg text-center cursor-pointer hover:scale-105 transition-transform" onclick="quickAchievement('weekly')">
                            <div class="text-lg">📈</div>
                            <div class="text-xs text-white font-bold">Semanal</div>
                        </div>
                        <div class="bg-gradient-to-br from-green-500 to-emerald-500 p-2 rounded-lg text-center cursor-pointer hover:scale-105 transition-transform" onclick="quickAchievement('team')">
                            <div class="text-lg">🤝</div>
                            <div class="text-xs text-white font-bold">Equipe</div>
                        </div>
                        <div class="bg-gradient-to-br from-purple-500 to-pink-500 p-2 rounded-lg text-center cursor-pointer hover:scale-105 transition-transform" onclick="quickAchievement('special')">
                            <div class="text-lg">⭐</div>
                            <div class="text-xs text-white font-bold">Especial</div>
                        </div>
                    </div>
                </div>

                <!-- Power-ups Disponíveis -->
                <div class="mt-4 pt-4 border-t border-purple-700">
                    <div class="text-xs text-purple-300 font-bold mb-3 text-center">⚡ POWER-UPS</div>
                    <div class="space-y-2">
                        <button onclick="usePowerUp('xp')" class="w-full bg-gradient-to-r from-blue-600 to-blue-700 hover:from-blue-500 hover:to-blue-600 text-white p-2 rounded-lg text-xs font-bold transition-all duration-200 hover:scale-105">
                            💎 Dobrar XP (50 moedas)
                        </button>
                        <button onclick="usePowerUp('time')" class="w-full bg-gradient-to-r from-green-600 to-green-700 hover:from-green-500 hover:to-green-600 text-white p-2 rounded-lg text-xs font-bold transition-all duration-200 hover:scale-105">
                            ⏰ Tempo Extra (30 moedas)
                        </button>
                        <button onclick="usePowerUp('luck')" class="w-full bg-gradient-to-r from-yellow-600 to-yellow-700 hover:from-yellow-500 hover:to-yellow-600 text-white p-2 rounded-lg text-xs font-bold transition-all duration-200 hover:scale-105">
                            🍀 Super Sorte (40 moedas)
                        </button>
                    </div>
                </div>
            </nav>
        </div>

        <!-- Conteúdo Principal -->
        <div id="mainContent" class="ml-64 transition-all duration-300">
            <!-- Header -->
            <header class="game-gradient shadow-lg border-b px-6 py-4">
                <div class="flex justify-between items-center">
                    <div class="flex items-center space-x-4">
                        <button id="toggleSidebar" class="text-white hover:text-yellow-300 text-xl">
                            ☰
                        </button>
                        <h1 class="text-2xl font-bold text-white">🎮 GamificaGestão - <span id="currentDept">Departamento</span></h1>
                    </div>
                   
                    <!-- Sistema de Gamificação -->
                    <div class="flex items-center space-x-6">
                        <!-- Nível e XP -->
                        <div class="bg-white bg-opacity-20 rounded-lg px-4 py-2 text-white">
                            <div class="flex items-center space-x-2">
                                <span class="level-badge text-xs px-2 py-1 rounded-full text-gray-800 font-bold">
                                    ⭐ Nível <span id="userLevel">1</span>
                                </span>
                                <div class="text-sm">
                                    <div class="flex items-center space-x-1">
                                        <span>💎</span>
                                        <span id="userXP">0</span>/<span id="nextLevelXP">100</span> XP
                                    </div>
                                    <div class="w-20 bg-white bg-opacity-30 rounded-full h-2 mt-1">
                                        <div id="xpBar" class="xp-bar h-2 rounded-full" style="width: 0%"></div>
                                    </div>
                                </div>
                            </div>
                        </div>
                       
                        <!-- Moedas -->
                        <div class="bg-white bg-opacity-20 rounded-lg px-3 py-2 text-white">
                            <div class="flex items-center space-x-1">
                                <span class="text-yellow-300">🪙</span>
                                <span id="userCoins" class="font-bold">0</span>
                            </div>
                        </div>
                       
                        <!-- Streak -->
                        <div class="bg-white bg-opacity-20 rounded-lg px-3 py-2 text-white">
                            <div class="flex items-center space-x-1">
                                <span class="streak-fire">🔥</span>
                                <span id="userStreak" class="font-bold">0</span>
                            </div>
                        </div>
                       
                        <button id="logoutFromMain" class="bg-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-600 transition duration-200">
                            🚪 Sair
                        </button>
                    </div>
                </div>
            </header>

            <!-- Barra de Tarefas do Mês -->
            <div class="px-6 py-4 bg-white border-b shadow-sm">
                <div class="flex justify-between items-center mb-6">
                    <div>
                        <h2 class="text-2xl font-bold text-gray-800 flex items-center space-x-2">
                            <span class="text-3xl">📅</span>
                            <span>Missões de <span id="currentMonth"></span></span>
                        </h2>
                        <p class="text-sm text-gray-600 mt-1">Departamento: <span id="currentDeptInTasks" class="font-semibold text-blue-600"></span></p>
                    </div>
                    <button id="addTaskBtn" class="bg-gradient-to-r from-purple-500 to-blue-600 text-white px-6 py-3 rounded-lg hover:from-purple-600 hover:to-blue-700 transition duration-200 font-bold shadow-lg transform hover:scale-105">
                        🎯 Nova Missão
                    </button>
                </div>
               
                <!-- Progresso Geral -->
                <div class="mb-6">
                    <div class="flex justify-between items-center mb-3">
                        <span class="text-lg font-semibold text-gray-800 flex items-center space-x-2">
                            <span class="text-xl">📊</span>
                            <span>Progresso Geral do Mês</span>
                        </span>
                        <div class="flex items-center space-x-4">
                            <span class="text-sm text-gray-600">
                                <span id="completedTasksCount">0</span> de <span id="totalTasksCount">0</span> missões
                            </span>
                            <span id="progressPercentage" class="text-2xl font-bold text-blue-600">0%</span>
                        </div>
                    </div>
                    <div class="w-full bg-gray-200 rounded-full h-4 shadow-inner">
                        <div id="progressBar" class="bg-gradient-to-r from-blue-500 to-green-500 h-4 rounded-full progress-bar transition-all duration-1000 ease-out" style="width: 0%"></div>
                    </div>
                </div>

                <!-- Lista de Todas as Missões do Departamento -->
                <div class="space-y-4">
                    <div class="flex items-center justify-between">
                        <h3 class="text-lg font-bold text-gray-800 flex items-center space-x-2">
                            <span class="text-xl">📅</span>
                            <span>Missões Semanais do Departamento</span>
                        </h3>
                        <div class="flex items-center space-x-4 text-sm">
                            <div class="flex items-center space-x-1">
                                <span class="w-3 h-3 bg-red-500 rounded-full"></span>
                                <span class="text-red-600 font-medium">Urgente</span>
                                <span id="urgentTasksCount" class="bg-red-500 text-white px-2 py-1 rounded-full text-xs font-bold">0</span>
                            </div>
                            <div class="flex items-center space-x-1">
                                <span class="w-3 h-3 bg-yellow-500 rounded-full"></span>
                                <span class="text-yellow-600 font-medium">Importante</span>
                                <span id="importantTasksCount" class="bg-yellow-500 text-white px-2 py-1 rounded-full text-xs font-bold">0</span>
                            </div>
                            <div class="flex items-center space-x-1">
                                <span class="w-3 h-3 bg-green-500 rounded-full"></span>
                                <span class="text-green-600 font-medium">Normal</span>
                                <span id="normalTasksCount" class="bg-green-500 text-white px-2 py-1 rounded-full text-xs font-bold">0</span>
                            </div>
                        </div>
                    </div>
                   
                    <!-- Área Única para Todas as Missões Semanais -->
                    <div class="bg-gradient-to-br from-gray-50 to-blue-50 rounded-xl p-6 border-2 border-gray-200 shadow-sm min-h-64">
                        <div class="mb-4 text-center">
                            <p class="text-sm text-gray-600">
                                💡 <strong>Dica:</strong> Clique em uma missão semanal para movê-la para suas tarefas diárias
                            </p>
                        </div>
                        <div id="allTasksList" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
                            <div class="col-span-full text-center text-gray-500 py-12">
                                <div class="text-6xl mb-4">📅</div>
                                <h4 class="text-xl font-bold text-gray-700 mb-2">Nenhuma missão semanal criada ainda</h4>
                                <p class="text-gray-600">Clique em "Nova Missão" para adicionar tarefas da semana!</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Seções de Tarefas -->
            <div class="p-6">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Tarefas Pendentes -->
                    <div class="bg-gradient-to-br from-red-50 to-red-100 rounded-xl p-4 border-2 border-red-200 shadow-lg">
                        <h3 class="text-lg font-semibold text-red-800 mb-4 flex items-center justify-between">
                            <span class="flex items-center space-x-2">
                                <span class="text-2xl">📋</span>
                                <span>Missões Pendentes</span>
                            </span>
                            <span class="bg-red-500 text-white px-3 py-1 rounded-full text-sm font-bold">
                                <span id="pendingCount">0</span>
                            </span>
                        </h3>
                        <div id="pendingTasks" class="space-y-3 min-h-32">
                            <div class="text-center text-gray-500 py-8">
                                <div class="text-4xl mb-2">🎯</div>
                                <p>Nenhuma missão pendente!</p>
                            </div>
                        </div>
