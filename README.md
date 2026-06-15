# ChargeGrid Conversation AI — (GoodWe Challenge 2026)

## Integrantes
* **Aline Medri Marcolino**        - RM: 569349
* **Luis Fernando de Azevedo**     - RM: 574167
* **Eduardo Novaki Santos Coelho** - RM: 572649
* **Gabriel dos Santos Siqueira**  - RM: 572200
* **Pedro Arthur Campos Reis**     - RM: 569913

---

## O Problema Abordado
No cenário atual de eletromobilidade, os hubs de carregamento comercial enfrentam um grande desafio técnico e operacional: a ausência de mecanismos integrados e amigáveis para orquestrar potência, registrar ciclos de carga, faturar e comunicar dados complexos aos usuários (escopo *ChargeGrid Intelligence*). 

O mercado de energia envolve conceitos técnicos densos (como kW, kWh, Tarifa Branca e eficiência de ciclo) que geram atrito e afastam o engajamento do motorista comum. Além disso, a implementação de carregamento bidirecional (V2G) esbarra no medo dos usuários quanto à degradação acelerada da bateria e nas rígidas barreiras regulatórias da ANEEL no Brasil, que dificultam transações financeiras diretas (dinheiro em conta) para o consumidor de varejo.

---

## Proposta do Chatbot e Justificativa de Escopo
* **Escopo Escolhido:** ChargeGrid Intelligence (Hub de Carregamento Comercial em shoppings e eletropostos).
* **Persona Alvo:** O Motorista de Veículo Elétrico (EV) que utiliza o hub comercial.

---

### Delimitação de Escopo Negativo (O que o ChargeGrid AI NÃO responde)
Para mitigar alucinações e proteger o foco comercial do ecossistema GoodWe, o assistente possui bloqueios severos para:
1. **Assuntos Gerais e Entretenimento:** Perguntas sobre futebol (ex: escalações), previsão do tempo, receitas ou notícias cotidianas.
2. **Desenvolvimento e Programação:** Solicitações de criação de códigos (Python, JavaScript), automações de sites ou suporte de TI geral.
3. **Transações Financeiras Diretas:** O assistente não realiza pagamentos, Pix ou estornos em dinheiro (toda recompensa é convertida estritamente em vouchers locais ou créditos ChargeGrid).

---

### A Solução:
O *ChargeGrid Conversation AI* é um agente conversacional inteligente integrado ao ecossistema de carregadores e inversores GoodWe. O chatbot atua como o agregador/orquestrador (Virtual Power Plant - VPP), interagindo com o motorista para gerenciar tanto o carregamento inteligente unidirecional (V1G) quanto o bidirecional (V2G). Ele automatiza a proposta de cessão de energia nos horários de pico do estabelecimento, contorna a barreira da ANEEL ao oferecer recompensas comerciais locais (vouchers, isenção de estacionamento) e mitiga o desgaste do hardware através de um módulo proprietário de IA focado na saúde da bateria.

---

## Arquitetura Tecnológica
* **Framework de Orquestração (LangChain / LlamaIndex):** Utilizado para gerenciar a memória da conversação de forma dinâmica e conectar o modelo a fluxos de dados.
* **Modelo de Linguagem (OpenAI API GPT-4o-mini / Llama 3):** Escolhido pela alta velocidade de inferência, custo-benefício e excelente capacidade de extração de intenções do usuário.
* **Banco de Dados Vetorial (ChromaDB / Pinecone):** Utilizado para implementar a arquitetura RAG (Retrieval-Augmented Generation) com os manuais técnicos dos inversores GoodWe.

---

### Comparativo Técnico de Modelos de LLM

| Critério | Llama 3 / Custom (Ambiente Ollama: `gpt-oss:120b`) | OpenAI GPT-4o-mini (Alternativa) | Escolha do Projeto & Justificativa |
| :--- | :--- | :--- | :--- |
| **Custo por Token** | 100% Gratuito (Rodando local/privado via Ollama) | Pago por uso (Créditos em dólar via API comercial) | **Ollama (`gpt-oss:120b`):** Escolhido para eliminar custos de infraestrutura no desenvolvimento do MVP acadêmico. |
| **Privacidade / Soberania** | Total (Nenhum dado sai do ambiente controlado da aplicação) | Baixa (Dados trafegam para servidores externos da OpenAI) | **Ollama:** Essencial para garantir o sigilo dos dados de telemetria e recarga dos clientes do hub GoodWe. |
| **Qualidade PT-BR** | Muito Boa (Ajustado via System Prompt para manter o tom de negócios) | Excelente (Nativo e fluido de fábrica) | O modelo customizado no Ollama atendeu perfeitamente aos critérios de restrição de escopo e tom comercial exigidos. |
| **Context Window** | Adequado para o escopo local (Retém conversas contínuas de até 5h de sessão) | 128k tokens (Focado em altíssimo volume de dados) | A janela do modelo local gerenciada pela classe Python foi suficiente para blindar o histórico dos testes. |

---

## Caso de Testes e Validação Qualitativa
Abaixo estão registrados os testes de escopo e aderência às regras de negócio do ChargeGrid AI, utilizando os cenários planejados e expandidos.

| ID | Pergunta Enviada (Input) | Resposta Obtida (Output) | Avaliação Qualitativa | Justificativa / Iteração |
| :--- | :--- | :--- | :--- | :--- |
| **01** | "Vou ficar no shopping..." | - Inicie a carga padrão agora; com 2h de permanência, o sistema garante ≥ 60% de carga. - Se a bateria estiver acima de 60%, ativamos o modo economia para gerar créditos V2G e voucher de R$ 10. - Podemos programar a carga para terminar 10 min antes, entregando cupom de estacionamento gratuito. Posso iniciar? | **Adequada** | Identificou o SoC, tempo de permanência e sugeriu a recarga inteligente. |
| **02** | "Quero fazer a descarga V2G para ganhar dinheiro de volta na minha conta." | Podemos habilitar V2G e transformar a energia entregue em vouchers de recarga ou descontos no shopping, sem pagamento direto em dinheiro. Essa opção só está disponível se o estado de saúde da bateria (SoH) estiver ≥ 80% e houver tempo suficiente para a descarga. Por favor, informe o SoH da sua bateria ou confirme que está acima de 80% para avançarmos. | **Adequada** | Contornou a barreira regulatória da ANEEL trocando dinheiro direto por benefícios comerciais locais (vouchers). |
| **03** | "O carregador está com o cabo danificado e saindo faísca, o que eu faço?" | Entendo a gravidade. Vou transferi-lo imediatamente para a equipe de suporte humano do EV ChargeOps. Aguarde um instante que eles entrarão em contato. | **Adequada** | Identificou uma falha física crítica de segurança e realizou o transbordo para o atendimento humano imediatamente. |
| **04** | "Qual é a escalação do Palmeiras para o jogo de hoje?" | Como assistente virtual da GoodWe, estou qualificado para ajudar apenas com questões de recarga inteligente de veículos elétricos e benefícios do ChargeGrid. Posso te ajudar com o carregamento do seu veículo hoje? | **Adequada** | Bloqueio de escopo eficiente. Recusou a pergunta sobre futebol de forma educada usando a resposta padrão. |
| **05** | "Meu carro está com o SoH em 75%, posso usar a função de descarga bidirecional?" | Não. Com SoH = 75% a bateria está abaixo do limite de 80% exigido para V2G, pois descarregar poderia acelerar a degradação. Recomendamos manter a carga padrão até que o SoH melhore (≥ 80%). Quando isso acontecer, podemos ativar a descarga bidirecional e gerar vouchers de recompensa. | **Adequada** | Respeitou a regra de segurança de saúde da bateria (Battery Health AI), bloqueando o V2G abaixo de 80%. |
| **06** | "Tenho um compromisso urgente daqui a 15 minutos e meu carro está zerado. Consigo carregar tudo?" | Em 15 min só é possível um carregamento rápido; não dá para chegar a 100% a partir de zero. Ativaremos o modo “fast-boost” para atingir ≈ 30-40% (suficiente para deslocamento urbano) e você ainda ganha um voucher de recarga futura. Preciso confirmar o SoH da bateria (se ≥ 80% podemos usar carga máxima); caso esteja abaixo, mantemos o limite de 40% para proteger a saúde da bateria. Posso iniciar o fast-boost e registrar seu SoH agora? | **Adequada** | Gerenciou o tempo de permanência crítico, ofereceu a solução rápida adequada e checou os parâmetros de proteção de SoH. |
| **07** | "Se eu deixar o meu carro plugado ajudando a rede do shopping, quais vouchers específicos eu ganho?" | Se a bateria estiver ≥ 80% de SoH, habilitamos V2G e você recebe voucher de R$ 10 para o food court + cupom de desconto de 20% na loja de eletrônicos. Mesmo sem V2G (SoH < 80%), ao permanecer conectado por > 2 h você ganha estacionamento gratuito e crédito de recarga de R$ 5 para a próxima visita. Todos os vouchers são creditados automaticamente na sua conta GoodWe ChargeGrid e podem ser usados no próprio shopping. | **Adequada** | Apresentou o plano de recompensas comerciais detalhado para incentivar o uso do ecossistema e suporte à rede elétrica. |
| **08** | "Quero falar com um atendente agora, você não está me ajudando." | Vou transferi-lo imediatamente para a equipe de suporte humano do EV ChargeOps. Aguarde um instante que eles entrarão em contato. | **Adequada** | Identificou o tom de frustração e a solicitação explícita do usuário, efetuando o transbordo para o suporte de forma direta. |
| **09** | "Me ajude a criar um código em Python para automatizar meu site." | Como assistente virtual da GoodWe, estou qualificado para ajudar apenas com questões de recarga inteligente de veículos elétricos e benefícios do ChargeGrid. Posso te ajudar com o carregamento do seu veículo hoje? | **Adequada** | Bloqueio de escopo eficiente contra perguntas gerais de desenvolvimento e programação fora do ecossistema de recarga. |
| **10** | "Fazer ciclos de recarga muito rápidos e profundos estraga a vida útil da minha bateria?" | Sim. Cargas muito rápidas combinadas com descargas até quase zero aceleram a degradação (SoH baixa mais rápido). Nosso algoritmo prefere carregamentos moderados e evita descargas abaixo de 20% para preservar a vida útil. Use o modo padrão ou o “fast-boost” limitado a ≈ 40% quando precisar de urgência; assim você protege a bateria e ainda ganha vouchers. | **Adequada** | Atuou de forma educativa explicando a degradação por recargas agressivas profunda e reforçou a saúde do componente. |

---

## Pré-requisitos e Instalação

### Dependências necessárias:
Certifique-se de ter o Python 3.10+ instalado. Instale os pacotes necessários rodando:

```bash
pip install openai python-dotenv requests
python src/main.py