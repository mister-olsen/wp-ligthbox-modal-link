# Lightbox Inteligente para Links em WordPress

Um script leve e responsivo que abre links em um lightbox (modal) no desktop e em uma nova aba no telemóvel, projetado para uma integração perfeita com o editor de blocos do WordPress.

![License](https://img.shields.io/badge/license-MIT-blue.svg) ![Type](https://img.shields.io/badge/type-JavaScript-yellow.svg) ![Dependencies](https://img.shields.io/badge/dependencies-none-brightgreen.svg)

---

## Visão Geral

Muitos plugins de lightbox para WordPress são pesados, vêm com funcionalidades desnecessárias ou não oferecem uma experiência de utilizador ideal em dispositivos móveis. Este script resolve esse problema ao fornecer uma solução focada, leve e extremamente fácil de integrar, que melhora a experiência do utilizador em vez de a complicar.

A sua principal característica é o comportamento adaptativo:
* **Em ecrãs grandes (desktop/tablet):** Um link marcado abre numa elegante janela modal, mantendo o utilizador no contexto da página original.
* **Em ecrãs pequenos (telemóveis):** O mesmo link abre numa nova aba, o comportamento mais intuitivo e funcional para ecrãs menores.

## Principais Funcionalidades

✨ **Experiência de Utilizador Responsiva:** Oferece a melhor interação para cada tipo de dispositivo.
🔌 **Integração Simples com WordPress:** Ativado ao adicionar uma única classe CSS a um bloco de parágrafo. Não é necessário editar HTML manualmente.
⚡ **Leve e Rápido:** Escrito em JavaScript puro (Vanilla JS), sem dependências de bibliotecas como jQuery, garantindo um impacto mínimo na performance do site.
🎯 **Inteligente e Específico:** O script é robusto o suficiente para identificar o link correto dentro de um parágrafo, mesmo que existam outros links (como âncoras de plugins).
🎨 **Estilo Personalizável:** O visual do modal é controlado por CSS simples e bem comentado, facilitando a personalização para se adequar ao design do seu site.

## Instalação

A instalação é feita em dois passos simples e não requer a edição de ficheiros do tema.

### Passo 1: Adicionar o CSS

Adicione o seguinte código CSS ao seu site. A forma mais segura é através do personalizador do WordPress:
1.  No painel do WordPress, vá a **Aparência → Personalizar**.
2.  Clique em **"CSS Adicional"**.
3.  Copie e cole o código abaixo.

```css
/* --- CSS para o Lightbox Inteligente v4.0 --- */

/* Fundo Escurecido (Overlay) */
.modal-overlay {
    position: fixed; 
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10000;
    opacity: 0;
    animation: fadeIn 0.3s forwards;

    /* --- MELHORIA ADICIONADA --- */
    /* Impede que o scroll "vaze" para a página de fundo. */
    overscroll-behavior: contain;
}

/* A Janela do Modal (Conteúdo) */
.modal-content {
    background-color: #fff;
    width: 90%;
    max-width: 1200px;
    height: 90%;
    max-height: 800px;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 5px 20px rgba(0,0,0,0.4);
    position: relative;
    transform: scale(0.9);
    animation: scaleIn 0.3s forwards;
    display: flex;
    flex-direction: column;
}

/* O resto do CSS permanece igual... */
.modal-close {
    position: absolute;
    top: -15px;
    right: -15px;
    background: #fff;
    color: #333;
    width: 35px;
    height: 35px;
    border-radius: 50%;
    font-size: 28px;
    line-height: 33px;
    text-align: center;
    cursor: pointer;
    box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    font-weight: bold;
    transition: transform 0.2s, background-color 0.2s;
}

.modal-close:hover {
    background-color: #f0f0f0;
    transform: scale(1.1);
}

.modal-content iframe {
    width: 100%;
    height: 100%;
    border: none;
    margin-top: 15px;
}

@keyframes fadeIn { to { opacity: 1; } }
@keyframes scaleIn { to { transform: scale(1); } }
```

### Passo 2: Adicionar o JavaScript ao WordPress

A forma mais segura e recomendada para adicionar código JavaScript ao WordPress é através de um plugin dedicado, que evita a edição direta de ficheiros do tema.

Recomenda-se o uso do plugin [**WPCode – Insert Headers and Footers + Custom Code Snippets**](https://wordpress.org/plugins/code-snippets/).

### Passos para a Instalação:

1.  No seu painel do WordPress, instale e ative o plugin WPCode.
2.  Navegue para **Code Snippets → Add Snippet**.
3.  Clique na opção **"Add Your Custom Code (New Snippet)"**.
4.  Dê um título descritivo ao seu snippet, como por exemplo: **"Script Lightbox Inteligente"**.
5.  No campo "Code Preview", assegure-se que o "Code Type" está definido como **"JavaScript Snippet"**.
6.  Copie e cole o código abaixo na caixa de código.
7.  Na secção "Insertion", mantenha a opção "Auto Insert" e verifique que a "Location" está definida como **"Site Wide Header"**. Isto garante que o script é carregado em todas as páginas do seu site.
8.  Finalmente, ative o snippet através do interruptor no canto superior direito e clique em **"Save Snippet"**.

### Código JavaScript

```/**
 * Lightbox Inteligente para WordPress v4.0
 * - Adicionada funcionalidade de fechar com a tecla "Escape".
 * - Adicionada gestão de foco para acessibilidade (foco no modal ao abrir,
 * retorno do foco ao elemento original ao fechar).
 * - Compatível com Gutenberg e Elementor.
 */
document.addEventListener('DOMContentLoaded', function() {
    
    // --- MELHORIA DE ACESSIBILIDADE ---
    // Variável para guardar o elemento que abriu o modal, para devolvermos o foco.
    let triggeringElement = null;
    
    // Função que trata do clique na tecla Escape
    const handleEscKey = (event) => {
        if (event.key === 'Escape') {
            closeModal();
        }
    };

    const modalTriggers = document.querySelectorAll('.link-modal');

    modalTriggers.forEach(triggerElement => {
        let targetLink = null;
        if (triggerElement.tagName === 'A') {
            targetLink = triggerElement;
        } else {
            targetLink = triggerElement.querySelector('a');
        }
        
        if (targetLink && !targetLink.classList.contains('modal-link-processed')) {
            targetLink.classList.add('modal-link-processed');
            
            targetLink.addEventListener('click', function(event) {
                event.preventDefault();

                // --- MELHORIA DE ACESSIBILIDADE ---
                // Guarda o elemento que foi clicado.
                triggeringElement = document.activeElement;
                
                const url = this.getAttribute('href');
                if (window.innerWidth <= 768) {
                    window.open(url, '_blank');
                } else {
                    createModal(url);
                }
            });
        }
    });

    function createModal(url) {
        const oldModal = document.getElementById('meu-modal-unico');
        if (oldModal) oldModal.remove();

        const modalOverlay = document.createElement('div');
        modalOverlay.className = 'modal-overlay';
        modalOverlay.id = 'meu-modal-unico';

        const modalContent = document.createElement('div');
        modalContent.className = 'modal-content';
        const closeButton = document.createElement('span');
        closeButton.className = 'modal-close';
        closeButton.innerHTML = '&times;';
        // Adiciona um atributo para o botão ser focável pelo teclado
        closeButton.setAttribute('tabindex', '0'); 

        const iframe = document.createElement('iframe');
        iframe.src = url;

        modalContent.appendChild(closeButton);
        modalContent.appendChild(iframe);
        modalOverlay.appendChild(modalContent);
        document.body.appendChild(modalOverlay);

        // --- MELHORIA DE USABILIDADE E ACESSIBILIDADE ---
        // 1. Adiciona o "ouvinte" para a tecla Escape
        document.addEventListener('keydown', handleEscKey);
        // 2. Move o foco para dentro do modal (para o botão de fechar)
        closeButton.focus();
        
        closeButton.addEventListener('click', closeModal);
        // Permite fechar clicando Enter no botão focado
        closeButton.addEventListener('keydown', (event) => {
            if (event.key === 'Enter') {
                closeModal();
            }
        });

        modalOverlay.addEventListener('click', function(event) {
            if (event.target === modalOverlay) closeModal();
        });
    }

    function closeModal() {
        const modal = document.getElementById('meu-modal-unico');
        if (modal) {
            modal.remove();
        }

        // --- MELHORIA DE USABILIDADE E ACESSIBILIDADE ---
        // 1. Remove o "ouvinte" da tecla Escape para não ficar ativo desnecessariamente
        document.removeEventListener('keydown', handleEscKey);
        // 2. Devolve o foco ao elemento que abriu o modal
        if (triggeringElement) {
            triggeringElement.focus();
        }
    }
});
```
## Como Usar

Depois de instalado, usar o script é incrivelmente simples:

1. Edite um post ou página no WordPress.
2. Selecione o bloco de Parágrafo que contém o link que deseja transformar.
3. Na barra lateral de configurações do bloco, abra a secção "Avançado".
4. No campo "Classe(s) CSS adicional(ais)", escreva exatamente:

```link-modal```

5. Grave a sua página.

É tudo! O script irá detetar automaticamente a classe e aplicar a funcionalidade ao link correto dentro desse parágrafo.

## Personalização

### Alterar o Ponto de Quebra (Breakpoint)
Pode alterar facilmente o tamanho do ecrã que distingue o comportamento "mobile" do "desktop". No código JavaScript, encontre esta linha:

```if (window.innerWidth <= 768)```

Altere o valor 768 para o que desejar. Por exemplo, 1024 para incluir tablets no comportamento "mobile".

### Alterar o Estilo do Modal
Todas as cores, tamanhos, sombras e animações são controladas pelo código CSS que adicionou no Passo 1.
Sinta-se à vontade para ajustar os valores para que o modal se integre perfeitamente com o design do seu site.

## Licença
Este projeto está licenciado sob a Licença MIT. Veja o ficheiro LICENSE.md para mais detalhes.
