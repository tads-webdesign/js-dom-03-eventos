# js-dom-03-eventos
Tutorial para Iniciantes: Tornando Páginas Web Interativas com Eventos

## Índice
1. [O que são Eventos](#o-que-são-eventos)
2. [Event Listeners: addEventListener](#event-listeners-addeventlistener)
3. [Tipos de Eventos](#tipos-de-eventos)
4. [Objeto Event e event.target](#objeto-event-e-eventtarget)
5. [preventDefault e stopPropagation](#preventdefault-e-stoppropagation)
6. [Validação de Formulários com JavaScript](#validação-de-formulários-com-javascript)
7. [Exemplo Completo](#exemplo-completo-formulário-interativo-com-validação)

---

## O que são Eventos

Eventos são ações ou ocorrências que acontecem no navegador. Eles podem ser disparados pelo usuário (como cliques, movimentos do mouse, teclas pressionadas) ou pelo próprio navegador (como o carregamento da página).

Os eventos permitem que você crie páginas web interativas, respondendo às ações dos usuários em tempo real.

**Exemplo simples:**
```javascript
// Quando o usuário clica em um botão, algo acontece
button.onclick = function() {
    alert('Botão clicado!');
};
```

---

## Event Listeners: addEventListener

O método `addEventListener()` é a forma moderna e recomendada de lidar com eventos em JavaScript. Ele permite adicionar múltiplos manipuladores para o mesmo evento e oferece mais controle.

**Sintaxe:**
```javascript
elemento.addEventListener(tipoDeEvento, função, opções);
```

**Exemplo:**
```html
<button id="meuBotao">Clique aqui</button>

<script>
const botao = document.getElementById('meuBotao');

botao.addEventListener('click', function() {
    console.log('Botão foi clicado!');
});

// Você pode adicionar múltiplos listeners para o mesmo evento
botao.addEventListener('click', function() {
    console.log('Segunda ação executada!');
});
</script>
```

**Vantagens do addEventListener:**
- Permite múltiplos manipuladores para o mesmo evento
- Pode ser removido com `removeEventListener()`
- Funciona em todos os navegadores modernos
- Suporta opções avançadas como `capture` e `once`

---

## Tipos de Eventos

Existem diversos tipos de eventos disponíveis. Aqui estão os mais comuns:

### 1. Click
Disparado quando o usuário clica em um elemento.

```html
<button id="btnClick">Clique em mim</button>

<script>
document.getElementById('btnClick').addEventListener('click', function() {
    alert('Você clicou no botão!');
});
</script>
```

### 2. Submit
Disparado quando um formulário é enviado.

```html
<form id="meuForm">
    <input type="text" name="nome" required>
    <button type="submit">Enviar</button>
</form>

<script>
document.getElementById('meuForm').addEventListener('submit', function(event) {
    event.preventDefault(); // Previne o envio padrão
    console.log('Formulário enviado!');
});
</script>
```

### 3. Keypress
Disparado quando uma tecla é pressionada. **Nota importante:** O evento `keypress` está obsoleto (deprecated). Prefira usar `keydown` ou `keyup` em código novo.

```html
<input type="text" id="campoTexto" placeholder="Digite algo">

<script>
// Recomendado: usar keydown em vez de keypress
document.getElementById('campoTexto').addEventListener('keydown', function(event) {
    console.log('Tecla pressionada: ' + event.key);
});
</script>
```

### 4. Mouseover
Disparado quando o mouse passa sobre um elemento.

```html
<div id="minhaDiv" style="width:200px; height:100px; background:lightblue;">
    Passe o mouse aqui
</div>

<script>
document.getElementById('minhaDiv').addEventListener('mouseover', function() {
    this.style.background = 'lightgreen';
});

document.getElementById('minhaDiv').addEventListener('mouseout', function() {
    this.style.background = 'lightblue';
});
</script>
```

### 5. Change
Disparado quando o valor de um elemento de formulário muda.

```html
<select id="meuSelect">
    <option value="">Selecione</option>
    <option value="opcao1">Opção 1</option>
    <option value="opcao2">Opção 2</option>
</select>

<script>
document.getElementById('meuSelect').addEventListener('change', function() {
    console.log('Valor selecionado: ' + this.value);
});
</script>
```

---

## Objeto Event e event.target

Quando um evento é disparado, um objeto `Event` é automaticamente passado para a função manipuladora. Este objeto contém informações sobre o evento.

**event.target** é uma propriedade importante que se refere ao elemento que disparou o evento.

**Exemplo:**
```html
<div id="container">
    <button class="btn">Botão 1</button>
    <button class="btn">Botão 2</button>
    <button class="btn">Botão 3</button>
</div>

<script>
document.getElementById('container').addEventListener('click', function(event) {
    // event.target é o elemento que foi clicado
    if (event.target.classList.contains('btn')) {
        console.log('Botão clicado: ' + event.target.textContent);
        event.target.style.background = 'yellow';
    }
});
</script>
```

**Propriedades úteis do objeto Event:**
- `event.type` - tipo do evento (ex: 'click', 'submit')
- `event.target` - elemento que disparou o evento
- `event.currentTarget` - elemento ao qual o listener está anexado
- `event.key` - tecla pressionada (para eventos de teclado)
- `event.clientX`, `event.clientY` - coordenadas do mouse

---

## preventDefault e stopPropagation

Estes são métodos importantes para controlar o comportamento de eventos.

### preventDefault()
Previne a ação padrão do navegador para aquele evento.

**Exemplo:**
```html
<a href="https://www.example.com" id="meuLink">Clique aqui</a>

<script>
document.getElementById('meuLink').addEventListener('click', function(event) {
    event.preventDefault(); // Impede a navegação
    console.log('Link clicado, mas não navegou!');
});
</script>
```

### stopPropagation()
Impede que o evento se propague (bubble) para elementos pais.

**Exemplo:**
```html
<div id="pai" style="padding: 20px; background: lightgray;">
    Div Pai
    <button id="filho">Botão Filho</button>
</div>

<script>
document.getElementById('pai').addEventListener('click', function() {
    console.log('Div pai clicada');
});

document.getElementById('filho').addEventListener('click', function(event) {
    event.stopPropagation(); // Impede que o evento chegue ao pai
    console.log('Botão filho clicado');
    // Apenas esta mensagem será exibida
});
</script>
```

---

## Validação de Formulários com JavaScript

A validação de formulários é essencial para garantir que os dados inseridos pelos usuários sejam corretos antes de serem processados.

**Exemplo de validação básica:**
```html
<form id="formCadastro">
    <input type="text" id="nome" placeholder="Nome completo">
    <span id="erroNome" style="color: red;"></span>
    
    <input type="email" id="email" placeholder="E-mail">
    <span id="erroEmail" style="color: red;"></span>
    
    <button type="submit">Cadastrar</button>
</form>

<script>
document.getElementById('formCadastro').addEventListener('submit', function(event) {
    event.preventDefault();
    
    // Limpar mensagens de erro
    document.getElementById('erroNome').textContent = '';
    document.getElementById('erroEmail').textContent = '';
    
    let valido = true;
    
    // Validar nome
    const nome = document.getElementById('nome').value.trim();
    if (nome.length < 3) {
        document.getElementById('erroNome').textContent = 'Nome deve ter pelo menos 3 caracteres';
        valido = false;
    }
    
    // Validar email
    const email = document.getElementById('email').value.trim();
    const regexEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!regexEmail.test(email)) {
        document.getElementById('erroEmail').textContent = 'E-mail inválido';
        valido = false;
    }
    
    if (valido) {
        console.log('Formulário válido! Enviando...');
        // Aqui você enviaria os dados
    }
});
</script>
```

---

## Exemplo Completo: Formulário Interativo com Validação

Aqui está um exemplo completo que combina todos os conceitos aprendidos:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulário Interativo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        
        input, select, textarea {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        
        input:focus, select:focus, textarea:focus {
            border-color: #4CAF50;
            outline: none;
        }
        
        .error {
            color: #f44336;
            font-size: 14px;
            margin-top: 5px;
            display: none;
        }
        
        .error.show {
            display: block;
        }
        
        .success {
            color: #4CAF50;
            font-size: 14px;
            margin-top: 5px;
        }
        
        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        
        button:hover {
            background-color: #45a049;
        }
        
        button:disabled {
            background-color: #cccccc;
            cursor: not-allowed;
        }
        
        #mensagemSucesso {
            background-color: #d4edda;
            border: 1px solid #c3e6cb;
            color: #155724;
            padding: 15px;
            border-radius: 4px;
            margin-top: 20px;
            display: none;
        }
        
        .char-count {
            font-size: 12px;
            color: #666;
            text-align: right;
        }
    </style>
</head>
<body>
    <h1>Formulário de Cadastro Interativo</h1>
    
    <form id="formularioCadastro">
        <div class="form-group">
            <label for="nome">Nome Completo:</label>
            <input type="text" id="nome" name="nome" placeholder="Digite seu nome completo">
            <div class="error" id="erroNome">Nome deve ter pelo menos 3 caracteres</div>
        </div>
        
        <div class="form-group">
            <label for="email">E-mail:</label>
            <input type="email" id="email" name="email" placeholder="seuemail@exemplo.com">
            <div class="error" id="erroEmail">E-mail inválido</div>
        </div>
        
        <div class="form-group">
            <label for="idade">Idade:</label>
            <input type="number" id="idade" name="idade" min="18" max="120">
            <div class="error" id="erroIdade">Idade deve ser entre 18 e 120 anos</div>
        </div>
        
        <div class="form-group">
            <label for="pais">País:</label>
            <select id="pais" name="pais">
                <option value="">Selecione um país</option>
                <option value="brasil">Brasil</option>
                <option value="portugal">Portugal</option>
                <option value="angola">Angola</option>
                <option value="mocambique">Moçambique</option>
            </select>
            <div class="error" id="erroPais">Por favor, selecione um país</div>
        </div>
        
        <div class="form-group">
            <label for="mensagem">Mensagem (máximo 200 caracteres):</label>
            <textarea id="mensagem" name="mensagem" rows="4" maxlength="200"></textarea>
            <div class="char-count" id="contadorCaracteres">0/200 caracteres</div>
            <div class="error" id="erroMensagem">Mensagem deve ter pelo menos 10 caracteres</div>
        </div>
        
        <div class="form-group">
            <label>
                <input type="checkbox" id="termos" name="termos">
                Aceito os termos e condições
            </label>
            <div class="error" id="erroTermos">Você deve aceitar os termos</div>
        </div>
        
        <button type="submit" id="btnEnviar">Cadastrar</button>
    </form>
    
    <div id="mensagemSucesso">
        <h3>Cadastro realizado com sucesso!</h3>
        <p id="dadosCadastro"></p>
    </div>
    
    <script>
        // Selecionar elementos do DOM
        const form = document.getElementById('formularioCadastro');
        const nome = document.getElementById('nome');
        const email = document.getElementById('email');
        const idade = document.getElementById('idade');
        const pais = document.getElementById('pais');
        const mensagem = document.getElementById('mensagem');
        const termos = document.getElementById('termos');
        const btnEnviar = document.getElementById('btnEnviar');
        const mensagemSucesso = document.getElementById('mensagemSucesso');
        const dadosCadastro = document.getElementById('dadosCadastro');
        
        // Contador de caracteres para o campo mensagem
        mensagem.addEventListener('keyup', function(event) {
            const contador = document.getElementById('contadorCaracteres');
            const tamanho = this.value.length;
            contador.textContent = `${tamanho}/200 caracteres`;
            
            if (tamanho > 180) {
                contador.style.color = '#f44336';
            } else {
                contador.style.color = '#666';
            }
        });
        
        // Validação em tempo real para o email
        email.addEventListener('blur', function() {
            validarEmail();
        });
        
        // Mudança de cor ao focar nos campos
        const inputs = document.querySelectorAll('input, select, textarea');
        inputs.forEach(input => {
            input.addEventListener('focus', function() {
                this.style.backgroundColor = '#f0f8ff';
            });
            
            input.addEventListener('blur', function() {
                this.style.backgroundColor = 'white';
            });
        });
        
        // Mostrar mensagem ao selecionar país
        pais.addEventListener('change', function() {
            if (this.value) {
                console.log('País selecionado: ' + this.value);
            }
        });
        
        // Função de validação de email
        function validarEmail() {
            const erroEmail = document.getElementById('erroEmail');
            const regexEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            
            if (email.value.trim() === '' || !regexEmail.test(email.value)) {
                erroEmail.classList.add('show');
                return false;
            } else {
                erroEmail.classList.remove('show');
                return true;
            }
        }
        
        // Função de validação de nome
        function validarNome() {
            const erroNome = document.getElementById('erroNome');
            
            if (nome.value.trim().length < 3) {
                erroNome.classList.add('show');
                return false;
            } else {
                erroNome.classList.remove('show');
                return true;
            }
        }
        
        // Função de validação de idade
        function validarIdade() {
            const erroIdade = document.getElementById('erroIdade');
            const idadeValor = parseInt(idade.value);
            
            if (isNaN(idadeValor) || idadeValor < 18 || idadeValor > 120) {
                erroIdade.classList.add('show');
                return false;
            } else {
                erroIdade.classList.remove('show');
                return true;
            }
        }
        
        // Função de validação de país
        function validarPais() {
            const erroPais = document.getElementById('erroPais');
            
            if (pais.value === '') {
                erroPais.classList.add('show');
                return false;
            } else {
                erroPais.classList.remove('show');
                return true;
            }
        }
        
        // Função de validação de mensagem
        function validarMensagem() {
            const erroMensagem = document.getElementById('erroMensagem');
            
            if (mensagem.value.trim().length < 10) {
                erroMensagem.classList.add('show');
                return false;
            } else {
                erroMensagem.classList.remove('show');
                return true;
            }
        }
        
        // Função de validação de termos
        function validarTermos() {
            const erroTermos = document.getElementById('erroTermos');
            
            if (!termos.checked) {
                erroTermos.classList.add('show');
                return false;
            } else {
                erroTermos.classList.remove('show');
                return true;
            }
        }
        
        // Evento de submit do formulário
        form.addEventListener('submit', function(event) {
            // Previne o envio padrão do formulário
            event.preventDefault();
            
            // Executar todas as validações
            const nomeValido = validarNome();
            const emailValido = validarEmail();
            const idadeValida = validarIdade();
            const paisValido = validarPais();
            const mensagemValida = validarMensagem();
            const termosValidos = validarTermos();
            
            // Verificar se todas as validações passaram
            if (nomeValido && emailValido && idadeValida && paisValido && mensagemValida && termosValidos) {
                // Desabilitar botão de envio
                btnEnviar.disabled = true;
                btnEnviar.textContent = 'Enviando...';
                
                // Simular envio (em um caso real, aqui seria feita uma requisição AJAX)
                setTimeout(function() {
                    // Ocultar formulário
                    form.style.display = 'none';
                    
                    // Exibir mensagem de sucesso com os dados
                    dadosCadastro.innerHTML = `
                        <strong>Nome:</strong> ${nome.value}<br>
                        <strong>E-mail:</strong> ${email.value}<br>
                        <strong>Idade:</strong> ${idade.value} anos<br>
                        <strong>País:</strong> ${pais.options[pais.selectedIndex].text}<br>
                        <strong>Mensagem:</strong> ${mensagem.value}
                    `;
                    mensagemSucesso.style.display = 'block';
                    
                    // Resetar botão
                    btnEnviar.disabled = false;
                    btnEnviar.textContent = 'Cadastrar';
                    
                    console.log('Formulário enviado com sucesso!');
                }, 1500);
            } else {
                console.log('Formulário contém erros. Por favor, corrija-os.');
            }
        });
        
        // Prevenir propagação de eventos em elementos específicos
        btnEnviar.addEventListener('click', function(event) {
            // Este evento não se propagará para elementos pais
            event.stopPropagation();
        });
    </script>
</body>
</html>
```

### Como usar este exemplo:

1. **Abra o arquivo** `exemplo-formulario.html` incluído neste repositório em um navegador web
2. **Ou copie o código** acima em um novo arquivo HTML (por exemplo, `meu-formulario.html`)
3. **Interaja com o formulário** para ver:
   - Validação em tempo real
   - Mudanças de cor ao focar nos campos
   - Contador de caracteres
   - Mensagens de erro
   - Prevenção do envio padrão
   - Simulação de envio de dados

### Conceitos aplicados no exemplo:

✅ **addEventListener** para múltiplos eventos  
✅ **Eventos**: click, submit, keyup, blur, focus, change  
✅ **event.target** para identificar elementos  
✅ **preventDefault()** para prevenir envio do formulário  
✅ **stopPropagation()** para controlar propagação de eventos  
✅ **Validação completa** de todos os campos  
✅ **Feedback visual** para o usuário  

---

## Conclusão

Dominar eventos em JavaScript é fundamental para criar páginas web interativas e responsivas. Este tutorial cobriu os conceitos essenciais, desde o básico até exemplos práticos de validação de formulários.

**Próximos passos:**
- Pratique modificando o exemplo fornecido
- Explore outros tipos de eventos (scroll, resize, load, etc.)
- Aprenda sobre event delegation para otimizar performance
- Estude sobre eventos customizados
- Aprenda a trabalhar com APIs assíncronas (fetch, AJAX)

**Recursos adicionais:**
- [MDN Web Docs - Events](https://developer.mozilla.org/pt-BR/docs/Web/Events)
- [MDN Web Docs - addEventListener](https://developer.mozilla.org/pt-BR/docs/Web/API/EventTarget/addEventListener)
- [MDN Web Docs - Event Reference](https://developer.mozilla.org/pt-BR/docs/Web/Events)

---

*Tutorial criado para fins educacionais - TADS WebDesign*
