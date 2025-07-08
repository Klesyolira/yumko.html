<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Yumeko 6G Pro+</title>
    <style>
        :root {
            --primary-color: #4a6cf7; /* Cor primária padrão */
            --background-color: #ffffff;
            --text-color: #333333;
            --card-background: #f8f9fa;
            --border-color: #e0e0e0;
            --success-color: #28a745;
            --danger-color: #dc3545;
            --warning-color: #ffc107;
            --info-color: #17a2b8;
        }

        .dark-theme {
            --background-color: #121212;
            --text-color: #f8f9fa;
            --card-background: #1e1e1e;
            --border-color: #333333;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--background-color);
            color: var(--text-color);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 0;
            border-bottom: 1px solid var(--border-color);
            margin-bottom: 20px;
        }

        .logo {
            font-size: 24px;
            font-weight: bold;
            color: var(--primary-color);
        }

        .menu-btn {
            background: none;
            border: none;
            font-size: 24px;
            color: var(--text-color);
            cursor: pointer;
        }

        .card {
            background: var(--card-background);
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        .btn {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            display: block;
            width: 100%;
            margin-bottom: 10px;
            text-align: center;
            transition: background-color 0.3s;
        }

        .btn:hover {
            opacity: 0.9;
        }

        .btn-success {
            background-color: var(--success-color);
        }

        .btn-danger {
            background-color: var(--danger-color);
        }

        .btn-warning {
            background-color: var(--warning-color);
        }

        .btn-info {
            background-color: var(--info-color);
        }

        .status {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .status-indicator {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background-color: var(--danger-color);
        }

        .status-indicator.connected {
            background-color: var(--success-color);
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background-color: var(--card-background);
            width: 90%;
            max-width: 500px;
            border-radius: 10px;
            padding: 20px;
            max-height: 90vh;
            overflow-y: auto;
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .modal-title {
            font-size: 20px;
            font-weight: bold;
        }

        .close-btn {
            background: none;
            border: none;
            font-size: 24px;
            color: var(--text-color);
            cursor: pointer;
        }

        .server-list, .payload-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
            gap: 10px;
            margin-bottom: 20px;
        }

        .server-item, .payload-item {
            background: var(--background-color);
            border: 1px solid var(--border-color);
            border-radius: 5px;
            padding: 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }

        .server-item:hover, .payload-item:hover {
            border-color: var(--primary-color);
            transform: translateY(-2px);
        }

        .server-item.selected, .payload-item.selected {
            border-color: var(--primary-color);
            background-color: rgba(74, 108, 247, 0.1);
        }

        .server-flag, .payload-flag {
            width: 40px;
            height: 40px;
            margin: 0 auto 10px;
            border-radius: 50%;
            background-size: cover;
            background-position: center;
        }

        .theme-selector {
            display: flex;
            justify-content: space-around;
            margin: 20px 0;
        }

        .theme-btn {
            padding: 10px;
            border-radius: 5px;
            border: 1px solid var(--border-color);
            background: var(--background-color);
            color: var(--text-color);
            cursor: pointer;
        }

        .theme-btn.active {
            border-color: var(--primary-color);
            background: rgba(74, 108, 247, 0.1);
        }

        .color-picker {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin: 20px 0;
        }

        .color-option {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid transparent;
        }

        .color-option.selected {
            border-color: var(--text-color);
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
        }

        .form-group input {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid var(--border-color);
            background: var(--background-color);
            color: var(--text-color);
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">Yumeko 6G Pro+</div>
            <button class="menu-btn" onclick="toggleMenu()">☰</button>
        </header>

        <div class="status">
            <div>Status: <span id="connectionStatus">Desconectado</span></div>
            <div class="status-indicator" id="statusIndicator"></div>
        </div>

        <div class="card">
            <button class="btn" id="vpnButton" onclick="toggleVPN()">Conectar</button>
            <button class="btn btn-info" onclick="showModal('serverModal')">Mudar Servidor</button>
            <button class="btn btn-info" onclick="showModal('payloadModal')">Mudar Payload</button>
            <button class="btn btn-warning" onclick="checkUser()">Verificar Usuário</button>
            <button class="btn btn-danger" onclick="switchToNativeMode()">Voltar para App</button>
        </div>

        <div class="card">
            <h3>Temas</h3>
            <div class="theme-selector">
                <button class="theme-btn" onclick="setTheme('light')">Claro</button>
                <button class="theme-btn" onclick="setTheme('dark')">Escuro</button>
            </div>
            <h3>Cores</h3>
            <div class="color-picker" id="colorPicker">
                <!-- As opções de cor serão preenchidas por JavaScript -->
            </div>
        </div>
    </div>

    <!-- Modal de Servidores -->
    <div class="modal" id="serverModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title">Selecione um Servidor</h2>
                <button class="close-btn" onclick="hideModal('serverModal')">&times;</button>
            </div>
            <div class="server-list" id="serverList">
                <!-- Servidores serão carregados aqui -->
            </div>
        </div>
    </div>

    <!-- Modal de Payloads -->
    <div class="modal" id="payloadModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title">Selecione uma Payload</h2>
                <button class="close-btn" onclick="hideModal('payloadModal')">&times;</button>
            </div>
            <div class="payload-list" id="payloadList">
                <!-- Payloads serão carregados aqui -->
            </div>
        </div>
    </div>

    <!-- Modal de Verificação de Usuário -->
    <div class="modal" id="userCheckModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title">Verificação de Usuário</h2>
                <button class="close-btn" onclick="hideModal('userCheckModal')">&times;</button>
            </div>
            <div id="userCheckContent">
                <div class="form-group">
                    <label for="username">Usuário</label>
                    <input type="text" id="username" value="">
                </div>
                <div class="form-group">
                    <label for="password">Senha</label>
                    <input type="password" id="password" value="">
                </div>
                <button class="btn" onclick="doCheckUser()">Verificar</button>
                <div id="userResult" style="margin-top: 20px;"></div>
            </div>
        </div>
    </div>

    <script>
        // Variáveis de estado
        let currentTheme = 'light';
        let currentColor = '#4a6cf7';
        let isConnected = false;
        let servers = [];
        let payloads = [];
        let selectedServerId = 0;
        let selectedPayloadId = 0;

        // Função para alternar a VPN
        function toggleVPN() {
            Android.startStopVPN();
        }

        // Função para verificar o usuário
        function checkUser() {
            Android.checkUser();
        }

        // Função para alternar para o modo nativo
        function switchToNativeMode() {
            Android.switchToNativeMode();
        }

        // Funções para mostrar/ocultar modais
        function showModal(modalId) {
            document.getElementById(modalId).style.display = 'flex';
        }

        function hideModal(modalId) {
            document.getElementById(modalId).style.display = 'none';
        }

        // Função para definir o tema
        function setTheme(theme) {
            currentTheme = theme;
            document.body.classList.toggle('dark-theme', theme === 'dark');
            // Salvar preferência (pode ser implementado com localStorage se necessário)
        }

        // Função para definir a cor
        function setColor(color) {
            currentColor = color;
            document.documentElement.style.setProperty('--primary-color', color);
            // Salvar preferência
        }

        // Função para carregar dados da configuração
        function carregarDados(config) {
            console.log('Dados recebidos:', config);

            // Atualizar estado da VPN
            isConnected = config.status === 'Conectado';
            document.getElementById('connectionStatus').textContent = isConnected ? 'Conectado' : 'Desconectado';
            document.getElementById('statusIndicator').className = isConnected ? 'status-indicator connected' : 'status-indicator';
            document.getElementById('vpnButton').textContent = isConnected ? 'Desconectar' : 'Conectar';
            document.getElementById('vpnButton').className = isConnected ? 'btn btn-danger' : 'btn btn-success';

            // Atualizar servidores
            servers = config.servers || [];
            selectedServerId = config.ServerPos || 0;
            renderServerList();

            // Atualizar payloads
            payloads = config.payloads || [];
            selectedPayloadId = config.PayloadPos || 0;
            renderPayloadList();

            // Atualizar campos de login
            document.getElementById('username').value = config.USER || '';
            document.getElementById('password').value = config.PASS || '';
        }

        // Função para renderizar a lista de servidores
        function renderServerList() {
            const serverList = document.getElementById('serverList');
            serverList.innerHTML = '';
            servers.forEach((server, index) => {
                const serverItem = document.createElement('div');
                serverItem.className = `server-item ${index === selectedServerId ? 'selected' : ''}`;
                serverItem.innerHTML = `
                    <div class="server-flag" style="background-image: url('${server.flagUrl}')"></div>
                    <div>${server.name}</div>
                `;
                serverItem.addEventListener('click', () => {
                    selectedServerId = index;
                    renderServerList();
                    Android.onServidorSelecionado(index, server.name, server.serverIP);
                    hideModal('serverModal');
                });
                serverList.appendChild(serverItem);
            });
        }

        // Função para renderizar a lista de payloads
        function renderPayloadList() {
            const payloadList = document.getElementById('payloadList');
            payloadList.innerHTML = '';
            payloads.forEach((payload, index) => {
                const payloadItem = document.createElement('div');
                payloadItem.className = `payload-item ${index === selectedPayloadId ? 'selected' : ''}`;
                payloadItem.innerHTML = `
                    <div class="payload-flag" style="background-image: url('${payload.flagUrl}')"></div>
                    <div>${payload.name}</div>
                `;
                payloadItem.addEventListener('click', () => {
                    selectedPayloadId = index;
                    renderPayloadList();
                    Android.onPayloadSelecionada(index, payload.name);
                    hideModal('payloadModal');
                });
                payloadList.appendChild(payloadItem);
            });
        }

        // Função para inicializar o seletor de cores
        function initColorPicker() {
            const colors = ['#4a6cf7', '#28a745', '#dc3545', '#ffc107', '#17a2b8', '#6f42c1', '#fd7e14', '#e83e8c'];
            const colorPicker = document.getElementById('colorPicker');
            colors.forEach(color => {
                const colorOption = document.createElement('div');
                colorOption.className = 'color-option';
                colorOption.style.backgroundColor = color;
                colorOption.addEventListener('click', () => {
                    setColor(color);
                    // Remover a classe selected de todos
                    document.querySelectorAll('.color-option').forEach(opt => {
                        opt.classList.remove('selected');
                    });
                    colorOption.classList.add('selected');
                });
                if (color === currentColor) {
                    colorOption.classList.add('selected');
                }
                colorPicker.appendChild(colorOption);
            });
        }

        // Função para realizar a verificação de usuário (chamada pelo botão dentro do modal)
        function doCheckUser() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            Android.onLogin(username, password); // Salva as credenciais
            Android.checkUser(); // Chama a verificação
        }

        // Função para exibir o resultado da verificação de usuário
        function showUserResult(result) {
            const userResult = document.getElementById('userResult');
            userResult.innerHTML = result;
        }

        // Inicialização
        document.addEventListener('DOMContentLoaded', () => {
            initColorPicker();
            // Solicitar os dados de configuração ao carregar
            Android.requisitarConfig();
        });

        // Evento para fechar o modal ao clicar fora
        window.addEventListener('click', (event) => {
            if (event.target.classList.contains('modal')) {
                event.target.style.display = 'none';
            }
        });
    </script>
</body>
</html>
