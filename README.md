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
/* Estilos para o Modal Inteligente */
.modal-overlay {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background-color: rgba(0, 0, 0, 0.75);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10000;
    opacity: 0;
    animation: fadeIn 0.3s forwards;
}
.modal-content {
    background-color: #fff;
    width: 90%; max-width: 1200px;
    height: 90%; max-height: 800px;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 5px 15px rgba(0,0,0,0.3);
    position: relative;
    transform: scale(0.9);
    animation: scaleIn 0.3s forwards;
    display: flex;
    flex-direction: column;
}
.modal-content iframe {
    width: 100%; height: 100%;
    border: none;
    margin-top: 15px;
}
.modal-close {
    position: absolute;
    top: -15px; right: -15px;
    background: #fff; color: #333;
    width: 35px; height: 35px;
    border-radius: 50%;
    font-size: 28px;
    line-height: 32px;
    text-align: center;
    cursor: pointer;
    box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    font-weight: bold;
}
.modal-close:hover { background: #f0f0f0; }
@keyframes fadeIn { to { opacity: 1; } }
@keyframes scaleIn { to { transform: scale(1); } }
