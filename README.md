
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Botões com Links Salvos</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f8f9fa;
        }
        
        h1 {
            color: #333;
            text-align: center;
            margin-bottom: 30px;
        }
        
        .button-container {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin: 25px 0;
            justify-content: center;
        }
        
        .custom-button {
            background-color: #4285f4;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 4px;
            cursor: pointer;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            transition: all 0.3s ease;
            position: relative;
        }
        
        .custom-button:hover {
            background-color: #2b6cb0;
            transform: translateY(-2px);
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        
        .button-actions {
            position: absolute;
            top: -10px;
            right: -10px;
            display: flex;
            gap: 5px;
        }
        
        .edit-button, .delete-button {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-weight: bold;
            font-size: 14px;
            border: none;
        }
        
        .edit-button {
            background-color: #ffd700;
            color: #333;
        }
        
        .delete-button {
            background-color: #ff4444;
            color: white;
        }
        
        .editor {
            background-color: white;
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 25px;
            margin-top: 30px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        .editor h2 {
            margin-top: 0;
            color: #333;
            border-bottom: 2px solid #f0f0f0;
            padding-bottom: 10px;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #555;
        }
        
        input[type="text"] {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 16px;
            transition: border-color 0.3s;
        }
        
        input[type="text"]:focus {
            border-color: #4285f4;
            outline: none;
        }
        
        .action-buttons {
            display: flex;
            gap: 10px;
            margin-top: 25px;
        }
        
        .action-button {
            padding: 12px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s ease;
        }
        
        .action-button:hover {
            transform: translateY(-2px);
        }
        
        .save-button {
            background-color: #4CAF50;
            color: white;
        }
        
        .clear-button {
            background-color: #f0f0f0;
            color: #333;
        }
        
        .status-message {
            margin-top: 20px;
            padding: 10px;
            border-radius: 4px;
            text-align: center;
            display: none;
        }
        
        .success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        
        .error {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
    </style>
</head>
<body>
    <h1>MEUS LINKS IMPORTANTES (POR EMERSON RODRIGUES)</h1>
    
    <div class="button-container" id="buttonContainer">
        <!-- Os botões serão adicionados aqui pelo JavaScript -->
    </div>
    
    <div class="editor">
        <h2 id="editorTitle">Adicionar Novo Botão</h2>
        
        <div class="form-group">
            <label for="buttonText">Texto do Botão:</label>
            <input type="text" id="buttonText" placeholder="Digite o texto que aparecerá no botão">
        </div>
        
        <div class="form-group">
            <label for="buttonLink">Link do Botão:</label>
            <input type="text" id="buttonLink" placeholder="Digite o URL (ex: https://www.google.com)">
        </div>
        
        <div class="action-buttons">
            <button class="action-button save-button" id="saveButton">Salvar Botão</button>
            <button class="action-button clear-button" id="clearButton">Limpar Campos</button>
        </div>
        
        <div id="statusMessage" class="status-message"></div>
    </div>
    
    <script>
        // Elementos DOM
        const buttonContainer = document.getElementById('buttonContainer');
        const editorTitle = document.getElementById('editorTitle');
        const buttonText = document.getElementById('buttonText');
        const buttonLink = document.getElementById('buttonLink');
        const saveButton = document.getElementById('saveButton');
        const clearButton = document.getElementById('clearButton');
        const statusMessage = document.getElementById('statusMessage');
        
        // Variáveis globais
        let buttons = [];
        let currentEditId = null;
        
        // Carregar botões do localStorage
        function loadButtons() {
            const savedButtons = localStorage.getItem('savedButtons');
            if (savedButtons) {
                buttons = JSON.parse(savedButtons);
                renderButtons();
            }
        }
        
        // Salvar botões no localStorage
        function saveButtons() {
            localStorage.setItem('savedButtons', JSON.stringify(buttons));
        }
        
        // Função para renderizar todos os botões
        function renderButtons() {
            buttonContainer.innerHTML = '';
            
            if (buttons.length === 0) {
                const message = document.createElement('p');
                message.textContent = 'Nenhum botão adicionado ainda. Use o formulário abaixo para criar botões.';
                message.style.color = '#666';
                message.style.textAlign = 'center';
                buttonContainer.appendChild(message);
                return;
            }
            
            buttons.forEach(button => {
                // Criar o elemento do botão
                const buttonElement = document.createElement('a');
                buttonElement.href = button.link;
                buttonElement.className = 'custom-button';
                buttonElement.textContent = button.text;
                buttonElement.target = '_blank'; // Abre em nova aba
                buttonElement.title = `Ir para: ${button.link}`;
                
                // Adicionar botões de ação
                const actionsContainer = document.createElement('div');
                actionsContainer.className = 'button-actions';
                
                // Botão de editar
                const editButton = document.createElement('button');
                editButton.className = 'edit-button';
                editButton.innerHTML = '✏️';
                editButton.title = 'Editar botão';
                editButton.onclick = function(e) {
                    e.preventDefault();
                    e.stopPropagation();
                    editButtonById(button.id);
                };
                
                // Botão de excluir
                const deleteButton = document.createElement('button');
                deleteButton.className = 'delete-button';
                deleteButton.innerHTML = '✖';
                deleteButton.title = 'Excluir botão';
                deleteButton.onclick = function(e) {
                    e.preventDefault();
                    e.stopPropagation();
                    deleteButtonById(button.id);
                };
                
                actionsContainer.appendChild(editButton);
                actionsContainer.appendChild(deleteButton);
                buttonElement.appendChild(actionsContainer);
                
                buttonContainer.appendChild(buttonElement);
            });
        }
        
        // Gerar ID único para novos botões
        function generateUniqueId() {
            if (buttons.length === 0) return 1;
            return Math.max(...buttons.map(b => b.id)) + 1;
        }
        
        // Mostrar mensagem de status
        function showStatusMessage(message, isError = false) {
            statusMessage.textContent = message;
            statusMessage.className = isError ? 'status-message error' : 'status-message success';
            statusMessage.style.display = 'block';
            
            // Esconder a mensagem após 3 segundos
            setTimeout(() => {
                statusMessage.style.display = 'none';
            }, 3000);
        }
        
        // Editar botão por ID
        function editButtonById(id) {
            const button = buttons.find(b => b.id === id);
            if (button) {
                buttonText.value = button.text;
                buttonLink.value = button.link;
                currentEditId = button.id;
                editorTitle.textContent = 'Editar Botão';
                saveButton.textContent = 'Atualizar Botão';
                
                // Rolar até o editor
                document.querySelector('.editor').scrollIntoView({ behavior: 'smooth' });
            }
        }
        
        // Excluir botão por ID
        function deleteButtonById(id) {
            if (confirm('Tem certeza que deseja excluir este botão?')) {
                buttons = buttons.filter(b => b.id !== id);
                saveButtons();
                renderButtons();
                showStatusMessage('Botão excluído com sucesso!');
                
                // Se estava editando este botão, limpar o editor
                if (currentEditId === id) {
                    clearEditor();
                }
            }
        }
        
        // Salvar ou atualizar botão
        function saveOrUpdateButton() {
            const text = buttonText.value.trim();
            let link = buttonLink.value.trim();
            
            // Validação básica
            if (!text) {
                showStatusMessage('Por favor, digite um texto para o botão.', true);
                return;
            }
            
            if (!link) {
                showStatusMessage('Por favor, digite um link para o botão.', true);
                return;
            }
            
            // Adicionar http:// se não existir protocolo
            if (!link.startsWith('http://') && !link.startsWith('https://')) {
                link = 'https://' + link;
            }
            
            if (currentEditId === null) {
                // Criar novo botão
                const newButton = {
                    id: generateUniqueId(),
                    text: text,
                    link: link
                };
                buttons.push(newButton);
                showStatusMessage('Novo botão adicionado com sucesso!');
            } else {
                // Atualizar botão existente
                const buttonIndex = buttons.findIndex(b => b.id === currentEditId);
                if (buttonIndex !== -1) {
                    buttons[buttonIndex].text = text;
                    buttons[buttonIndex].link = link;
                    showStatusMessage('Botão atualizado com sucesso!');
                }
            }
            
            // Salvar no localStorage e atualizar interface
            saveButtons();
            renderButtons();
            clearEditor();
        }
        
        // Limpar editor
        function clearEditor() {
            buttonText.value = '';
            buttonLink.value = '';
            currentEditId = null;
            editorTitle.textContent = 'Adicionar Novo Botão';
            saveButton.textContent = 'Salvar Botão';
        }
        
        // Configurar event listeners
        saveButton.addEventListener('click', saveOrUpdateButton);
        clearButton.addEventListener('click', clearEditor);
        
        // Inicialização
        document.addEventListener('DOMContentLoaded', function() {
            loadButtons();
            
            // Adicionar alguns botões de exemplo se não houver nenhum
            if (buttons.length === 0) {
                buttons = [
                    { id: 1, text: "Google", link: "https://emerson1993rc.github.io/Ferramentas/caixa_copiar_demanda_livre.html" },
                    { id: 2, text: "YouTube", link: "https://www.youtube.com" },
                    { id: 3, text: "Wikipedia", link: "https://www.wikipedia.org" }
                ];
                saveButtons();
                renderButtons();
            }
        });
    </script>
</body>
</html>
