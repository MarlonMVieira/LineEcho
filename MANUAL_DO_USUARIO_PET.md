# 🐾 Manual do Usuário — Pet  (LineEcho OS)

> [!IMPORTANT]
> **REGRA DE MANUTENÇÃO OBRIGATÓRIA:**  
> Este manual deve ser **rigorosamente atualizado a cada alteração de código ou nova funcionalidade** implementada no projeto. Ele serve como documentação de referência direta tanto para os desenvolvedores quanto para o usuário final e para a assistência por voz do assistente.

---

## 🌟 1. O que é o Pet?

O **Pet ** é o assistente virtual vivo e evolutivo integrado ao **LineEcho**. Ele une dois grandes conceitos:
1. **O Pet Virtual Inteligente (Inspiração no Tamagotchi / Pestinha):** possui rosto expressivo com animações vetoriais aceleradas por hardware, pisca naturalmente, reage a carinho na tela, acumula Afeição (❤️) e Pontos de Experiência (XP) e evolui de nível com o tempo.
2. **O Agente Autônomo para Android (Inspiração no Open):** é capaz de navegar pela interface do Android, abrir aplicativos, clicar em botões, preencher campos de texto e **aprender novas tarefas de forma autônoma ou por observação**, criando novos **Neurônios** permanentes no seu cérebro.

---

## 🎭 2. Emoções e Interações Físicas (Cafuné e Evolução)

### Expressões do Pet:
* **Sereno (IDLE):** Olhos azuis ciano neon, piscadas naturais aleatórias (a cada 2.5s a 6s) e respiração suave.
* **Ouvindo (LISTENING):** Olhos atentos com anel de escuta ativado ao ouvir a palavra de ativação (*""* ou *"Echo"*).
* **Pensando / Raciocinando (THINKING):** Olhos violeta/magenta enquanto consulta a IA ou deduz uma sequência de automação.
* **Feliz / Sorrindo (HAPPY):** Olhos em arco sorridente no estilo `^ ^` com bochechas rosadas ao receber carinho ou concluir tarefas.
* **Lente de Câmera (LOOKING_CAMERA):** Anel de foco avermelhado quando solicitado para ver algo via câmera.
* **Descanso / Sonolento (SLEEPY):** Pálpebras semi-fechadas no modo relógio de cabeceira.

### Mecânica de Carinho e Interação (Cafuné e Arraste):
* **Cafuné na Tela:** Deslize o dedo suavemente sobre os olhos do Pet. Ele solta uma expressão de alegria em arco `(^ ^)` com bochechas coradas, ganha **+5 de Afeição (❤️)** e **+2 de XP**.
* **Brincar / Arrastar o Pet:** Ao arrastar o avatar do Pet pela tela, ele reage alegremente ao movimento com expressão sorridente e carinho (+2 Afeição, +1 XP), comemorando o estímulo de brincadeira. As bochechas contam com espaçamento e respiro geométrico dedicado para nunca encostarem nos olhos.

---

## 🧠 3. O Painel Neural

O **Painel Neural** é a central onde você visualiza e gerencia tudo o que o Pet  sabe e aprendeu.

### Como Abrir o Painel Neural:
1. **Com 1 Toque:** Dê um toque simples na face do Pet  na tela principal.
2. **Por Voz:** Diga *", abrir painel neural"*, *", mostrar neurônios"* ou *", o que você aprendeu?"*.
3. **Pelas Configurações:** Abra a engrenagem e toque em *"🧠 Ver Neurônios & Painel Neural"*.

### O que você encontra no Painel:
* **Barra de XP e Nível:** Acompanhe os pontos restantes para o próximo nível (*Filhote ➔ Curioso ➔ Esperto ➔ Assistente ➔  Mestre*).
* **Identidade do Dono & Reconhecimento Facial:**
  * Nome do dono memorizado (com botão `[ ✎ Alterar ]`).
  * Indicador de rosto gravado via câmera com miniatura da foto capturada e botão `[ 📸 Olhar e Gravar Rosto ]`.
* **Lista de Habilidades e Neurônios:**
  * Todos os neurônios gravados com seus respectivos gatilhos de voz e passos na tela.
  * Botão **`[ ▶ Testar ]`**: Executa a rotina na hora para validação direta no aparelho.
  * Botão **`[ ✏️ Ajustar ]` (Estilo MacroDroid)**: Abre o **Editor Visual de Passos**, permitindo:
    * Ajustar milissegundos de espera (delays) entre cada passo (com botões rápidos `-500ms`, `+500ms`, `+1s`).
    * Editar alvos de texto, IDs de botões ou coordenadas físicas `(X, Y)`.
    * Reordenar a ordem dos passos (▲ / ▼).
    * Adicionar novas pausas ou cliques.
    * Alterar a frase ou expressão de voz de ativação.
  * Botão **`[ ✕ Esquecer ]`**: Deleta uma habilidade indesejada do cérebro.
* **Fatos & Memórias Gravadas:** Fatos memorizados sobre o dono e preferências, com botão `[ + Adicionar Fato ]`.

---

## 🎓 4. Como Ensinar Coisas Novas para o Pet 

O Pet  conta com **2 métodos revolucionários de aprendizado**:

### Método A: Aprendizado por Demonstração ("Modo Vigia") — *Mais Fácil e Visual!*
Neste modo, você simplesmente mostra para o Pet como fazer na tela, e ele copia e memoriza para sempre:
1. Diga: **", aprender"** (ou *", me observe"* / *", aprenda comigo"*).
2. O Pet ativará o badge **● GRAVANDO** e exibirá a barra assistiva no topo da tela.
3. Abra o aplicativo que você deseja ensinar (ex: Player de Música, Mercado Livre, Shopee, Telegram, etc.) e faça os passos com naturalidade:
   * **Temporização Real Respeitada:** O tempo que você esperar entre um toque e outro (ex: aguardar o player abrir até a tela de reprodução ou botão Play carregar) é registrado com exatidão pelo assistente (suporta esperas de até 30 segundos).
   * **Espera Dinâmica Inteligente:** Durante a execução, se o aplicativo demorar um pouco mais para carregar em determinado dia, o motor do assistente faz retentativas automáticas e inteligentes (polling de acessibilidade) de até 2.5s para localizar o botão dinamicamente no instante em que ele surge!
4. Quando terminar os passos na tela, diga: **", terminei"** (ou *", pronto"* / toque em [Terminei] na barra flutuante).
5. O  compila a sequência inteira, gera um **Novo Neurônio Permanente** (+50 XP) e comemora! A partir daí, basta pedir para ele fazer aquela tarefa. Se quiser calibrar tempos ou botões depois, basta usar o botão **`[ ✏️ Ajustar ]`** no Painel Neural.

### Método B: Aprendizado Autônomo por Voz via IA (Open)
Você pede o que quer em linguagem natural e a IA (ExperimentalLabs) deduz os passos:
* Exemplo: *", pesquisa capinha na Shopee"*
* Exemplo: *", busca ração no Mercado Livre"*
* Exemplo: *", abre a Play Store e pesquisa Telegram"*
* **Como funciona:** Ele abre o app sozinho, inspeciona todos os botões e campos de busca pela árvore de acessibilidade do Android, envia para a central de IA deduzir os passos, executa na tela e grava o novo neurônio permanente no cérebro.

---

## 🎈 5. Pet  Flutuante (Livre na Tela sobre Outros Apps)

Você pode deixar o Pet  solto na tela flutuando por cima de qualquer jogo, vídeo ou aplicativo:

* **Como Ativar:** Diga *", ativar pet flutuante"* ou toque no botão nas Configurações.
* **Como Movimentar:** Arraste a cabecinha do Pet para qualquer canto da tela com o dedo.
* **Toque no Pet Flutuante:** Abre um mini-menu radial com atalhos para:
  * `[ 🧠 ]` Abrir Painel Neural imediatamente.
  * `[ ⏺ / ⏹ ]` Iniciar ou finalizar o Modo Vigia de Aprendizado.
  * `[ ✕ ]` Ocultar o Pet Flutuante.
* **Como Desativar:** Diga *", ocultar pet flutuante"*.

---

## 👁 6. Visão Computacional e Gravação de Rosto Sob Demanda

> [!NOTE]
> **Privacidade Absoluta:** A câmera frontal **NUNCA** fica aberta em segundo plano. Ela só acorda quando você pede expressamente e fecha imediatamente após a foto (consumo zero de bateria).
> No Amazon Echo Show, certifique-se de que a **trava física deslizante da câmera** (o botão deslizante no topo do aparelho) esteja aberta (se houver um ponto laranja sobre a lente, ela está fisicamente bloqueada). As permissões do sistema (`CAMERA` e `appops`) são concedidas automaticamente e de forma silenciosa via Superusuário/Root.

### Comandos de Visão:
* **Gravar o Rosto do Dono:** Diga *", meu nome é Marlon e grava meu rosto"* ou *", memorize meu rosto"*.
  * Ele abre a câmera por 1 segundo, fotografa o rosto, grava o arquivo `owner_face.jpg` e cria os neurônios de **Reconhecimento Facial** e **Identidade do Dono** (+100 XP).
* **Perguntar quem é você:** *", quem sou eu?"* ou *", você me reconhece?"*.
* **Analisar Objetos:** Aponte algo para a câmera e diga *", o que você está vendo?"* ou *", veja isso"*. Ele usa a IA multimodal para descrever o objeto em áudio.

---

## ⚙️ 7. Configuração da Inteligência Artificial (ExperimentalLabs)

O Pet  vem configurado para utilizar o provedor **ExperimentalLabs** por meio da central FreeLLMAPI local:

* **Acesso:** Ícone de Engrenagem ➔ Card **"CÉREBRO DE IA & EXPERIMENTAL LABS"**.
* **Endpoint Padrão:** `http://192.168.25.24:3001/v1/chat/completions` (IP do PC onde o servidor FreeLLMAPI está ativo).
* **Chave de API:** `freellmapi-38f8e69761ee9578aac984daeb1bf6d7abceb2f062896f22`
* **Modelo:** `auto` (seleciona dinamicamente os melhores modelos como `qwen3.5-9b` e `deepseek-v4-flash`).
* **Botão "⚡ Testar ExperimentalLabs":** Envia um ping para testar a comunicação. Ao responder, o Pet  comemora por voz e tela.

---

## 🗣 8. Síntese de Voz Neural Ultra-Humana (OpenAI TTS-1)

O assistente conta com tecnologia de síntese de voz neural de nível estúdio transmitida pelo endpoint de IA do ExperientialLabs:
* **Camila (Feminina):** Utiliza o modelo neural `nova` (voz doce, cristalina, ultra-fluida e expressiva).
* **Ricardo (Masculino):** Utiliza o modelo neural `onyx` (voz encorpada, aveludada, imersiva e natural).
* **Configuração:** Acesse as Configurações (ícone de engrenagem) e selecione o perfil de voz desejado entre **Camila** e **Ricardo**. Ambas operam sem o tom mecânico ou robotizado de sintetizadores convencionais.

---

## 💬 9. Memória Persistente de Conversas e Gerenciamento

Todas as conversas e bate-papos realizados com o Pet  são armazenados com persistência local criptografada e vinculados ao contexto do modelo de IA.

### Como Visualizar o Histórico de Conversas:
1. **Pelo Painel Neural:** Abra o Painel Neural e toque no botão **[ 💬 Ver Conversas ]** localizado na seção de Memórias.
2. **Por Comando de Voz:** Diga *", ver conversas"*, *", mostrar histórico de conversas"* ou *", abrir memória de conversas"*.

### Como Gerenciar e Apagar Conversas:
* **Apagar Conversas de Hoje:** Diga *", apagar conversas de hoje"* (ou toque em `[ 📅 Apagar de Hoje ]` no diálogo).
* **Apagar Última Conversa:** Diga *", apagar última conversa"* (ou toque em `[ 🗑️ Apagar Última ]`).
* **Apagar Diálogo Individual:** No diálogo de histórico, toque no botão `[ ✕ ]` vermelho ao lado de qualquer mensagem.
* **Limpar Toda a Memória:** Diga *", limpar memória de conversas"* ou toque em `[ ⚠️ Limpar Tudo ]`.

---

## 🎙️ 10. Modo Conversa Contínua (Hands-Free)

Permite conversar livremente com o Pet  de forma ininterrupta, sem precisar repetir o nome dele a cada fala:
* **Como Ativar:** Diga *", conversar"* (ou *"Echo, conversar"*).
* **Comportamento:** O assistente ativa o anel de escuta com detecção de fala automática (VAD). A detecção de palavras de ativação (*wake word*) fica suspensa durante a conversa contínua para evitar ativações acidentais.
* **Como Parar:** A qualquer momento, basta dizer *"parar"*, *"tchau"*, *"cancelar"* ou *"chega"*.

---

## 📋 11. Tabela Rápida de Comandos de Voz

| Comando de Voz | Ação Executada |
| :--- | :--- |
| *", conversar"* | Ativa o modo de conversa contínua hands-free. |
| *"Parar"* / *"Cancelar"* | Encerra o modo conversa contínua e retorna à escuta padrão. |
| *", ver conversas"* | Abre a tela do histórico de conversas gravadas. |
| *", apagar conversas de hoje"* | Exclui todos os diálogos ocorridos na data atual. |
| *", apagar última conversa"* | Exclui o diálogo mais recente da memória. |
| *", limpar memória de conversas"* | Apaga todo o histórico persistente de conversas. |
| *", aprender"* | Ativa o Modo Observador / Vigia para gravar passos na tela. |
| *", terminei"* | Encerra a gravação da demonstração e gera o novo neurônio. |
| *", abrir painel neural"* | Exibe o Painel Neural com lista de neurônios, XP e memórias. |
| *", ativar pet flutuante"* | Faz o Pet  flutuar sobre qualquer aplicativo do Android. |
| *", ocultar pet flutuante"* | Esconde o avatar flutuante. |
| *", grava meu rosto"* | Abre a câmera frontal, grava o rosto e cria neurônio facial. |
| *", quem sou eu?"* | Responde o nome do dono e confirma reconhecimento. |
| *", veja isso"* | Analisa com a câmera frontal o objeto apontado. |
| *", pesquisa [termo] no [app]"* | Abre o app e deduz autonomamente como buscar o item. |
| *", [qualquer pergunta]"* | Consulta a IA e responde com voz ultra-humana fluida. |
| *", volume no 8"* / *"Volume 50%"* | Ajusta o volume do sistema Android. |
| *", tocar música"* | Abre o Spotify / reprodutor de música vinculado. |
| *", modo descanso"* | Entra no modo relógio noturno minimalista. |

---

## 👂 12. Palavras de Ativação (Wake Words) e Modo Cozinha Anti-Ruído

Você pode escolher como chamar o assistente e ajustar o nível de sensibilidade do microfone nas Configurações (ícone de engrenagem):

### Opções de Palavra de Ativação:
1. **Echo (Padrão):**
   - Utiliza o modelo acústico `echo.pmdl` treinado para o LineEcho.
   - Calibrado com ganho unitário (`1.0x`) e filtro adaptativo especial para eliminar disparos falsos com barulho de pratos, talheres, copos e ruídos de cozinha.
2. **Alexa (Modelo Universal):**
   - Utiliza a rede neural universal `alexa.umdl` (2.25 MB) extraída de assistentes de terceiros.
   - Funciona para qualquer voz (homem, mulher ou criança) sem necessidade de treinamento e com altíssima imunidade a ruídos da casa.
3. **Ziggy:**
   - Para ativar com a palavra "Ziggy", é necessário gravar 3 amostras de voz executando o script `treinar.bat` no computador. Enquanto novas amostras não forem gravadas, o modelo responde à palavra *"Pet"*.

### Níveis de Sensibilidade do Microfone:
* **Estrita (Recomendado para Cozinha e Ambientes Barulhentos):**
  - Sensibilidade `0.42` e atenuação de ganho `0.95x`.
  - Rejeita qualquer ruído mecânico contínuo (geladeira, exaustor, torneira) e descarta impactos agudos de talheres/louça.
* **Equilibrada (Padrão):**
  - Sensibilidade `0.46` e ganho `1.00x`.
  - Ideal para uso diário na sala ou bancada.
* **Alta:**
  - Sensibilidade `0.50` e ganho `1.06x`.
  - Recomendado apenas para ambientes silenciosos (quarto, escritório) para escutar a longas distâncias.

---

## ⚡ 13. Sincronização Vocal, Casa Inteligente e Conversa Contínua

### 🎭 1. Sincronismo Exato de Gestos e Fala (0ms Delay)
- **Expressão Simultânea:** O Pet inicia a gesticulação corporal (leve inclinação de cabeça de `3.5°` e direcionamento de olhar atento) no **mesmo instante exato** em que o áudio vocal sai no alto-falante.
- **Movimento Labial Orgânico:** A boca se movimenta em sincronia direta com a fala sintetizada via hardware, eliminando pausas ou gesticulações desincronizadas antes do som começar.
- **Suavização Suave:** Após os primeiros 700ms de gesticulação expressiva, a inclinação recentraliza suavemente enquanto a fala continua orgânica até o fim da frase.

### 🏠 2. Prioridade Máxima para Casa Inteligente (Tuya / Smart Life)
- **Resposta Instantânea:** Comandos de automação residencial (como *"apagar a luz da sala"*, *"ligar o ventilador"*, *"abrir a cortina"*) e **Rotinas Inteligentes estilo Alexa** (ex: *"modo cinema"*, *"rotina bom dia"*, *"hora de dormir"*) são posicionados no topo absoluto de processamento do despachante.
- **Sem Interrupções:** Não há conflito com alarmes, timers ou pesquisas web; o assistente identifica comandos de casa inteligente instantaneamente.

### 💬 3. Janela Dinâmica de Conversa Contínua (10 Segundos)
- **Diálogo Fluido sem Repetir a Palavra-Chave:** Após o assistente responder a qualquer pergunta ou comando falado, ele entra automaticamente em modo de escuta ativa por **10 segundos** (`VOICE_SILENCE_TIMEOUT_MS = 10000L`).
- **Como Funciona:** Você pode fazer perguntas complementares diretamente (ex: *"Qual o clima hoje?"* ➔ Pet responde ➔ *"E amanhã?"*), sem precisar chamá-lo novamente por "Echo" ou "Atlas".
- **Encerramento Rápido:** Caso você deseje encerrar imediatamente, basta dizer comandos naturais como *"tchau"*, *"valeu"*, *"obrigado"*, *"pode descansar"* ou *"vai dormir"*, ou aguardar os 10 segundos de silêncio para que ele retorne ao descanso normal automaticamente.


