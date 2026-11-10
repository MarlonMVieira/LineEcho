# Manual de Treinamento da IA e Habilidades Autônomas do LineEcho

Este documento é a especificação oficial de arquitetura, percepção visual, interação tátil e aprendizado autônomo do assistente **LineEcho Pet Jarvis**. Ele serve como guia permanente para evitar desvios no desenvolvimento e consolidar as diretrizes de autonomia do assistente.

---

## 1. Visão Geral da Arquitetura do Agente

O LineEcho opera através de uma integração entre:
1. **Serviço de Acessibilidade (`EchoAccessibilityService`)**: Fornece percepção em tempo real da árvore de elementos visuais do Android (Accessibility Node Hierarchy), sem consumir bateria com gravação contínua de tela e com latência praticamente zero.
2. **Cérebro LLM (`PetLLMClient`)**: Conecta-se à API (FreeLLMAPI / ExperientialLabs / OpenAI compatible) para raciocínio contextual, interpretação de intenções e tomada de decisão.
3. **Motor Executor (`PetAgentExecutor`)**: Converte decisões de alto nível em ações do Android (abertura de apps, toques dinâmicos, digitação, keyevents e gestos).
4. **Cérebro Neural (`PetBrain` & `PetNeuron`)**: Armazena habilidades aprendidas, memórias do usuário e rotinas com pausas reproduzíveis.
5. **Sincronização em Nuvem (`LineEchoBackupManager` & `GoogleDriveBackupClient`)**: Sincroniza o cérebro neural entre dispositivos sem destruir backups históricos.

---

## 2. Percepção Visual e Leitura de Tela

A IA possui olhos para o sistema através da árvore de nós da janela ativa:

- **Identificação do App Ativo**: Detecta o pacote (`getPackageName()`) e o rótulo amigável do aplicativo em primeiro plano (ex: Netflix, YouTube, Spotify, Configurações).
- **Detecção de Perfis de Usuário**: Em apps de streaming (Netflix, Prime Video, etc.), filtra os nomes de perfis visíveis na tela, permitindo à IA listar "Quem está assistindo" e selecionar perfis pelo nome falado.
- **Detecção de Campos de Entrada**: Identifica campos editáveis (`isEditable()`), campos de busca (`SearchAutoComplete`) e campos de senha/PIN (`isPassword()`).
- **Resumo Verbal para o Usuário**: Quando o usuário pergunta *"O que você está vendo na tela?"* ou *"Quem está na tela?"*, a IA responde de forma concisa e natural com base nos textos e perfis detectados.

---

## 3. Filosofia de Interação: Dynamic-First, Coordinate-Fallback

Para garantir robustez entre diferentes resoluções e layouts de tela (Echo Show 5, celulares Motorola, tablets ou TVs):

1. **Tentativa Dinâmica (Prioridade 1)**:
   - Busca nós por **texto exato ou parcial**, **ID de recurso** (`viewIdResourceName`) ou **descrição de conteúdo** (`contentDescription`).
   - Se o nó não for diretamente clicável, sobe na árvore de nós até encontrar o container clicável pai (ex: card ou botão pai).
   - Realiza toque via `AccessibilityNodeInfo.ACTION_CLICK` ou disparo de gesto físico no centro dos limites (`bounds.centerX(), bounds.centerY()`).
   - Aguarda até 2,5 segundos com repetições caso o app esteja abrindo ou renderizando animações.

2. **Fallback de Coordenadas Proporcionais (Prioridade 2)**:
   - Se o elemento dinâmico não for encontrado após a tolerância, o sistema recorre às coordenadas `(X, Y)`.
   - As coordenadas são ajustadas proporcionalmente à resolução do dispositivo atual caso tenham sido capturadas em outra resolução:
     $$\text{X}_{\text{alvo}} = \frac{\text{X}_{\text{origem}} \times \text{Largura}_{\text{atual}}}{\text{Largura}_{\text{origem}}}$$
     $$\text{Y}_{\text{alvo}} = \frac{\text{Y}_{\text{origem}} \times \text{Altura}_{\text{atual}}}{\text{Altura}_{\text{origem}}}$$

---

## 4. Manipulação de Campos de Texto e Teclados Virtuais

- **Campos de Busca e Texto Geral**:
  - Quando a LLM decide preencher um campo, ela emite a tag `[ACTION:TYPE:texto_a_digitar]` ou `[ACTION:TYPE:nome_do_campo|texto_a_digitar]`.
  - O `PetAgentExecutor` localiza o campo, ativa o foco, executa clique para posicionar o cursor e tenta preencher via `ACTION_SET_TEXT`.
  - Se o app for um aplicativo de TV, WebView ou teclado customizado que recusa `ACTION_SET_TEXT`, o executor injeta diretamente via shell root (`input text ...`).
  - Para confirmar a busca ou formulário, emite `[ACTION:PRESS:ENTER]` ou localiza botões como "Pesquisar", "Buscar", "Concluir" ou "OK".

---

## 5. Senha em Bloco e Teclado Numérico (PIN Loop)

Em aplicativos como Netflix ou apps bancários, campos de PIN frequentemente consistem em **4 caixas individuais separadas** ou uma grade de teclado numérico na tela:

### Comportamento da IA:
1. **Identificação de Senha em Bloco**:
   - Detecta telas de PIN (presença de "PIN", "Bloqueio de perfil", ou múltiplos blocos consecutivos de 1 caractere).
2. **Pergunta Proativa**:
   - Se o usuário ainda não tiver fornecido a senha, a IA pergunta amigavelmente:
     *"Detectei um campo de senha em bloco. Você gostaria que eu digitasse o PIN para você?"*
3. **Digitação em Loop com Pausas Humanas**:
   - Quando o usuário confirma e informa os números (ex: "1234"), a IA aciona a tag `[ACTION:PIN:1234]`.
   - O motor executa um **loop dígito por dígito**:
     - Para cada número:
       - **Passo A**: Verifica se há na tela botões numéricos virtuais com o dígito (ex: teclado numérico do app) e clica nele.
       - **Passo B (Fallback)**: Se não houver botão na tela, injeta o evento de teclado correspondente (`KEYCODE_0` a `KEYCODE_9`, códigos 7 a 16).
       - **Passo C (Pausa)**: Aguarda **350ms** entre cada dígito para que a interface do app processe o avanço automático para o próximo bloco.

---

## 6. Aprendizado Autônomo e Criação de Neurônios

O assistente é capaz de transformar qualquer sequência de ações realizadas pelo usuário em uma rotina permanente (**Neurônio**):

### Gatilho de Descanso (Silence Timeout):
- Quando o usuário termina de interagir e fica em silêncio por 10 segundos, o assistente verifica se foram executadas ações na tela durante aquela sessão de conversa.
- Se houver ações registradas (`PetAgentExecutor.hasSessionActions()`):
  - Em vez de adormecer imediatamente, a assistente pergunta por voz:
    *"Percebi que realizamos ações na tela. Você quer que eu salve esse caminho como um novo aprendizado?"*
  - Se o usuário responder **"Sim" / "Salva" / "Pode salvar" / "Quero"**:
    - O assistente sintetiza um novo `PetNeuron` preservando:
      - O app lançado;
      - Os cliques em botões e elementos;
      - Os textos e PINs digitados;
      - **As pausas temporizadas reais** entre as telas (para dar tempo de carregar);
    - Salva o neurônio no `PetBrain` (+50 XP para o Pet!);
    - Comemora por voz e finaliza.
  - Se o usuário responder **"Não" / "Cancela"**:
    - Descarta o histórico e entra em descanso sem salvar.
  - Se o usuário não disser nada no próximo timeout:
    - Entra em descanso silenciosamente.

### Gatilho Explícito por Voz:
- A qualquer momento o usuário pode dizer: *"Jarvis, memoriza esse caminho"*, *"Salva o que a gente fez"* ou *"Aprende esse caminho"*.
- O assistente empacota imediatamente o caminho percorrido em um novo neurônio.

---

## 7. Arquitetura de Backup na Nuvem (Google Drive)

Para que o assistente funcione perfeitamente entre múltiplos computadores e dispositivos sem perda de dados:

1. **Apenas 1 Arquivo por Mês no Drive**:
   - Nome padronizado: `LineEcho_Backup_YYYY_MM.json` (ex: `LineEcho_Backup_2026_11.json`).
   - Se já existir backup daquele mês, a API do Drive realiza atualização **PATCH in-place** do arquivo existente, preservando o ID e o histórico de versões.
2. **Nunca Deletar Backups Antigos da Nuvem**:
   - Backups de meses anteriores são mantidos permanentemente na pasta `LineEcho` do Google Drive do usuário. Nenhuma exclusão automática é executada.
3. **Sincronização Baseada na Data de Upload (`modifiedTime`)**:
   - O dispositivo lê o campo oficial de data de upload/modificação do Google Drive (`modifiedTime`).
   - Se o timestamp da nuvem for mais recente que o local, ele atualiza o cérebro do dispositivo.
   - Se o dispositivo já tiver a versão com aquela data, não realiza downloads nem re-uploads desnecessários.
4. **Separação de Memória Geral e Memória Neurônio**:
   - **Memória Geral (`memories`)**: Fatos pessoais do dono, conversas, preferências (independente de resolução de tela).
   - **Memória Neurônio (`neurons`)**: Habilidades e macros com seletores dinâmicos prioritários e metadados de resolução do aparelho que gerou o backup.

---

## 8. Regras de Rede e Canais Wi-Fi no Android (Qualcomm Prima / WCNSS)

Para que o assistente e os dispositivos de teste físicos (ex: Motorola Moto G / falcon) mantenham conectividade contínua com a rede Wi-Fi local:

1. **Canais Wi-Fi 10, 11, 12 e 13 Sempre Ativos (2.4 GHz)**:
   - **Problema de fábrica**: Por padrão, o driver Qualcomm Prima / LineageOS pode vir com os canais 10, 11, 12 e 13 desabilitados (`disabled`), escaneando apenas de 1 a 9. Se o roteador operar no Canal 11 (o mais comum no Brasil), o aparelho não detecta a rede (`COMPOSICAO`).
   - **Configuração Obrigatória no Sistema**:
     - `/system/build.prop`: `ro.wifi.channels=13` (nunca deixar vazio).
     - `/system/vendor/firmware/wlan/prima/WCNSS_qcom_cfg.ini`:
       ```ini
       g11dSupportEnabled=0
       gEnableBypass11d=1
       gAPChannelSelectEndChannel=13
       gAPCntryCode=BR
       gCountryCodePriority=1
       ```
2. **Requisitos de Framework para Varredura**:
   - `location_mode`: Deve estar em `3` (Alta precisão), pois o Android 9+ oculta redes Wi-Fi da interface e dos apps se a localização estiver desligada.
   - `wifi_scan_always_enabled`: `1`.
   - `auto_time` e `auto_time_zone`: `1` (relógio sincronizado é mandatório para validação WPA2/TLS).
3. **Limitação de Hardware**:
   - O chipset Qualcomm Snapdragon 400 (WCN3620) do Motorola Moto G 1ª Geração possui exclusivamente rádio de 2.4 GHz (802.11 b/g/n). Redes de 5 GHz (`COMP_5G`) são fisicamente incompatíveis e não devem ser buscadas neste modelo.

---

## 9. Treinador de Voz Dinâmico e Contínuo (Hands-Free Wake Word)

Para garantir máxima precisão acústica dos modelos neurais Snowboy (`ziggy.pmdl` e `custom.pmdl`), o processo de calibração de voz adota o fluxo contínuo e automatizado:

1. **Eliminação do Manuseio Físico**:
   - O usuário não precisa tocar na tela entre uma amostra e outra, evitando ruídos de atrito de dedos e permitindo manter a **distância ideal de fala (40cm a 60cm)**.
2. **Contagem Regressiva Guiada (3, 2, 1)**:
   - Antes de cada uma das 3 amostras, o display exibe uma contagem visual em destaque (`3` ➔ `2` ➔ `1`) acompanhada de bips sonoros suaves (`ToneGenerator`).
   - Ao término do 1, o aviso muda para `🎙️ FALE AGORA!` em vermelho e grava por 2,5 segundos.
3. **Encadeamento Automático**:
   - Amostra 1 gravada ➔ Confirmação sonora (ACK) e badge `1. OK ✓` ativado.
   - Pausa confortável de 1,5s para respiração.
   - Contagem (3, 2, 1) da Amostra 2 ➔ Gravação da Amostra 2 ➔ `2. OK ✓`.
   - Pausa confortável de 1,5s.
   - Contagem (3, 2, 1) da Amostra 3 ➔ Gravação da Amostra 3 ➔ `3. OK ✓`.
4. **Compilação e Ativação Instantânea**:
   - Ao concluir a 3ª amostra, o pacote é enviado automaticamente para a API neural na nuvem, o `.pmdl` é gerado e ativado no motor de hotword, com backup automático das amostras no Google Drive.

---

## 10. Desbloqueio de Permissões Restritas no Modelo Lite (Android 13+ / One UI)

Em versões modernas do Android (Android 13 Tiramisu, Android 14 UpsideDownCake e Android 15 VanillaIceCream) — especialmente na interface **Samsung One UI** e **Xiaomi HyperOS** —, aplicativos instalados manualmente fora da Google Play Store (sideloaded via APK) têm serviços sensíveis bloqueados pelo sistema de segurança operacional, gerando o aviso:

> **"Acesso negado ao app"**
> *O acesso a essa permissão pode colocar suas informações pessoais e financeiras em risco.*

Para o modelo **LineEcho Lite** (que não possui acesso Root para auto-concessão via shell) funcionar de forma 100% autônoma (lendo telas, simulando toques e digitando texto), implementamos o diálogo didático interativo `AccessibilityGuideDialog`:

1. **Passo 1: Liberar Restrição do Sistema (Informações do App)**:
   - O app dispara o intent `Settings.ACTION_APPLICATION_DETAILS_SETTINGS` para `package:com.lineecho.deskclock`.
   - O usuário toca nos **3 pontinhos (⋮)** no canto superior direito da tela de detalhes.
   - Seleciona **"Permitir configurações restritas"** e autentica com biometria ou PIN de bloqueio do aparelho.
2. **Passo 2: Ativar o Serviço de Acessibilidade**:
   - O app abre a tela de Acessibilidade via `Settings.ACTION_ACCESSIBILITY_SETTINGS`.
   - O usuário acessa **"Aplicativos instalados"** (ou "Serviços instalados") ➔ **"LineEcho"** ➔ Ativa a chave para **Ligado**.
3. **Monitoramento e Validação em Tempo Real**:
   - O `AccessibilityGuideDialog` mantém um observador em segundo plano que detecta instantaneamente quando `EchoAccessibilityService.isConnected()` se torna verdadeiro.
   - Assim que o usuário retorna ao aplicativo, o status atualiza automaticamente para `✅ Acessibilidade ATIVA e Conectada!` com botão de conclusão em destaque.

---

## 11. Identidade Sonora e Efeitos de Áudio do Pet

O LineEcho conta com uma camada sonora personalizada para tornar as interações táteis e comandos de voz suaves e acolhedores:

1. **Som de Carinho ao Passar a Mão no Pet (`Hum.mp3`)**:
   - Localizado em `SonsAssitent/Hum.mp3` e empacotado em `res/raw/hum.mp3`.
   - Gerenciado pela classe `PetSoundManager`:
     - Disparado automaticamente ao acariciar o rosto do Pet (`PetFaceView.triggerHappyCelebration()` / `MainActivity.onPetPetted()`) ou tocar/arrastar o Pet flutuante (`FloatingPetManager`).
     - Possui filtro de debounce de 1,4s para impedir sobreposição de áudio em toques rápidos e contínuos.
     - Utiliza `SoundPool` de baixa latência com fallback transparente para `MediaPlayer`.
2. **Som Suave de Ativação por Voz (`AtivacaoDeVoz.mp3`)**:
   - Localizado em `SonsAssitent/AtivacaoDeVoz.mp3` e convertido para PCM 16-bit 44.1kHz (`res/raw/ding.wav` e `assets/snowboy/`).
   - Substitui o antigo bip agudo por um sino suave de confirmação de escuta.
   - Disparado em `EchoWakeWordEngine.playDing()` ao detectar as palavras de ativação (*"Echo"*, *"Jarvis"*, *"Ziggy"* ou palavra customizada).




