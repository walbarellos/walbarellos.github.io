---
title: "Transcrição de Vídeos em Python"
date: 2025-05-06
tags: ["Python", "data-extraction", "Transcrição","Script"]
description: "Projeto Inspirado no preço cobrado para se transcrever um vídeo. Link e Comandos no post.."
weight: 1
draft: false
---

📖 Leia-me - Transcrição do YouTube com destaque e exportação
📝 Objetivo

Este script Python captura as legendas de um vídeo do YouTube usando a API de transcrição automática, agrupa as legendas em frases mais legíveis, destaca palavras-chave com cores no terminal e exporta a transcrição para um arquivo .txt formatado.

🛠 Pré-requisitos

Antes de rodar o programa, você precisará de:

    Python 3.x instalado. 
    
(Certifique-se de ter a versão mais recente do Python instalada no seu sistema).

Dependências do projeto:

        youtube-transcript-api
        
(para acessar as transcrições dos vídeos do YouTube)


Para instalar as dependências, execute:

```
    pip install youtube-transcript-api

```
🧑‍💻 Comandos para rodar o programa

Se estiver usando um ambiente virtual (venv), ative-o e execute:


```

➜ python3 -m venv venv # Crie o Ambiente
➜ source venv/bin/activate # Ative o ambiente
➜ pip install youtube-transcript-api (Instale dentro do ambiente para evitar conflitos com o Sistema)

```
Execute o script:

Após instalar as dependências, basta rodar o script com o seguinte comando no terminal:

python transcript.py

Forneça a URL do vídeo do YouTube:

Após rodar o script, será solicitado para que você cole a URL do vídeo do YouTube, algo como:

    Cole a URL do vídeo do YouTube: https://www.youtube.com/watch?v=example

📂 Saídas do Programa

![illu](https://github.com/user-attachments/assets/a9624582-7b23-40ab-b899-ddc0172a8a2f)

🎨 Personalização

Palavras-chave para destacar: Você pode adicionar ou remover palavras-chave no código (na lista palavras_chave) para destacar mais termos relevantes.

palavras_chave = ["Tutorial", "Teste", "Maça","Tumbleweed"]

Alterar número de frases exibidas: O script exibe as primeiras 10 frases no terminal. Se quiser aumentar ou diminuir esse número, modifique a linha no código:

    print_frases_coloridas(frases[:10], palavras_chave)

⚠ Limitações

    O script depende das legendas automáticas do YouTube, o que significa que a qualidade da transcrição pode variar.

    O script não realiza correções automáticas nas transcrições (como remoção de palavras erradas ou não capturadas).

    Caso o vídeo não tenha legendas, o programa exibirá uma mensagem de erro.

💡 Possíveis Melhorias Futuras

    Suporte para múltiplos idiomas.

    Resumo automático das transcrições.

    Integração com sistemas de recomendação para vídeos baseados em palavras-chave.
