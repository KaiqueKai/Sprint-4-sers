# Gestão Inteligente de Energia Solar com Armazenamento

Simulação em Python de uma residência com painéis fotovoltaicos, bateria e inversor híbrido nos moldes dos equipamentos **Goodwe**, comparando uma estratégia padrão de uso da bateria com uma estratégia inteligente.

## Equipe

| Integrante | RM |
|---|---|
| André Debiazzi Baldi Negrini | 569062 |
| Kaique da Silva Assis | 572718 |
| Vinicius Cristal de Oliveira | 572049 |
| Lucas Barros | 571528 |
| Renan Eskildssen | 571097 |

**Professor:** André Tritiack
**Disciplina:** [preencher]

## O que o projeto faz

O programa responde a uma pergunta prática: um sistema de gestão inteligente reduz o custo, o uso da rede e as emissões de CO2 mais do que um sistema que só usa a bateria quando falta energia?

1. **Cria um cenário simulado:** gera, hora a hora, a geração solar (com dias nublados e ensolarados) e o consumo da casa.
2. **Simula duas estratégias:**
   - **Padrão:** a bateria carrega com a sobra solar e descarrega sempre que falta energia.
   - **Inteligente:** reserva energia para o horário de ponta (18h às 21h, tarifa mais cara) e, de madrugada, completa a reserva pela rede somente se a previsão solar do dia seguinte for insuficiente.
3. **Calcula indicadores:** custo, energia importada e exportada, autossuficiência, autoconsumo e emissões de CO2.
4. **Mostra os resultados:** gera um painel de gráficos e um assistente virtual que responde perguntas simples.

## Como executar

Requisitos: Python 3.10 ou superior.

```bash
pip install -r requirements.txt
python goodwe_smart_energy.py
```

Opções úteis:

```bash
# Simular 30 dias e fazer uma pergunta ao assistente
python goodwe_smart_energy.py --dias 30 --perguntar "quanto economizei?"

# (Opcional, não testado) ler dados de um inversor Goodwe real na rede local
pip install goodwe
python goodwe_smart_energy.py --host 192.168.0.50
```

Os arquivos são gerados na pasta `saida/`:

- `kpis.csv`: indicadores comparativos
- `resultados.csv`: série horária de cada estratégia
- `dashboard.png`: painel de gráficos

## Estrutura do código

O arquivo `goodwe_smart_energy.py` está dividido em seções numeradas:

| Seção | Função |
|---|---|
| 1) Parâmetros (`Config`) | Painéis, bateria, tarifas e fator de emissão |
| 2) Dados simulados (`gerar_perfis`) | Geração solar e consumo hora a hora |
| 3) Simulação (`simular`) | Balanço de energia com as duas estratégias |
| 4) Indicadores (`calcular_kpis`, `sem_solar`) | Custo, autossuficiência, autoconsumo e CO2 |
| 5) Visualização (`gerar_dashboard`) | Painel de gráficos |
| 6) Assistente virtual (`assistente`) | Respostas em linguagem natural, baseadas em regras |
| 7) Conector Goodwe (`ler_inversor_real`) | Leitura de inversor real (opcional) |
| 8) Principal (`main`) | Executa tudo e salva os arquivos |

## Resultados (simulação de 30 dias)

| Indicador | Sem solar | Padrão | Inteligente |
|---|---|---|---|
| Energia importada (kWh) | 543,04 | 126,86 | 127,33 |
| Autossuficiência (%) | 0,0 | 76,6 | 76,5 |
| Custo de energia (R$) | 568,67 | 45,49 | 44,91 |
| Emissões da rede (kgCO2) | 32,58 | 7,61 | 7,64 |

Com solar e bateria, o custo cai cerca de 92% e as emissões associadas à rede cerca de 77% em relação a uma casa sem solar. A estratégia inteligente economiza apenas R$ 0,58 a mais que a padrão neste cenário. Os valores podem mudar ao alterar os parâmetros em `Config`.

## Limitações

- Os dados são **simulados**: não há medições reais de painéis, inversor ou consumo.
- Tarifas e fator de emissão (0,06 kgCO2/kWh) são **hipóteses editáveis**; devem ser trocados pelos valores da distribuidora e pelo fator oficial do ano analisado.
- A previsão solar usada pela estratégia inteligente é perfeita; em um sistema real viria de uma API meteorológica ou de uma plataforma como o SEMS+.
- O modelo usa passo de 1 hora, eficiência constante da bateria e não considera degradação nem custo de investimento.
- O conector com inversor Goodwe real não foi testado com equipamento.
- O assistente virtual é baseado em regras simples.

## Próximos passos

- Testar o conector com um inversor Goodwe real e comparar dados medidos com os simulados.
- Usar previsão meteorológica real e a tarifa da distribuidora.
- Incluir análise de retorno financeiro (payback).
- Integrar o assistente a Alexa, Google Assistant ou um modelo de linguagem.

## Tecnologias

Python 3, NumPy, pandas, Matplotlib e, opcionalmente, a biblioteca comunitária `goodwe`. Referência de equipamento e plataforma: inversor híbrido e bateria Goodwe, SEMS Portal / SEMS+.

## Estrutura sugerida do repositório

```
.
├── goodwe_smart_energy.py
├── requirements.txt
├── README.md
├── saida/            # kpis.csv, resultados.csv, dashboard.png
└── docs/             # PDF do projeto, diagrama de arquitetura, roteiro do vídeo
```
