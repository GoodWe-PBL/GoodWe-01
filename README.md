# GoodWe-01

Documentação de referência para integração e operação do carregador **GoodWe HCA G2** no contexto regulatório brasileiro.

---

## Resolução Normativa ANEEL nº 1.000/2021

A [Resolução Normativa ANEEL nº 1.000, de 7 de dezembro de 2021](https://www.gov.br/aneel/pt-br/assuntos/noticias/2022/conheca-a-resolucao-1-000-que-reune-os-direitos-e-deveres-do-consumidor-de-energia-eletrica) consolida as Regras de Prestação do Serviço Público de Distribuição de Energia Elétrica. Ela **revogou a RN 819/2018** (primeira norma específica sobre recarga de veículos elétricos) e incorporou o tema no **Capítulo V — Das Instalações de Recarga de Veículos Elétricos**.

A ANEEL adotou uma **regulamentação mínima**, com foco em evitar interferências na operação da rede e em impedir que a tarifa dos demais consumidores seja impactada pela atividade de recarga. As regras abaixo são as mais relevantes para projetos que envolvam exploração comercial e integração de plataformas de gestão.

### Definição de estação de recarga

**Estação de recarga** (Art. 2º, inciso XV): conjunto de softwares e equipamentos utilizados para o fornecimento de corrente alternada ou contínua ao veículo elétrico, instalado em um ou mais invólucros, com funções especiais de controle e de comunicação, e localizados fora do veículo.

### Exploração comercial

| Artigo | Disposição |
|--------|------------|
| **Art. 554** | É permitida a recarga de veículos elétricos que **não sejam do titular** da unidade consumidora em que se encontra a estação, **inclusive para fins de exploração comercial a preços livremente negociados**. |
| **Art. 557 a 560** | A distribuidora **pode**, a seu critério, prestar a atividade de recarga em sua área de atuação; os preços podem ser livremente negociados quando houver cobrança. |

Em outras palavras: qualquer interessado (condomínio, shopping, posto, empresa etc.) pode operar pontos de recarga acessíveis a terceiros e cobrar pelo serviço, sem que a ANEEL fixe teto de preço. Para isso, porém, é necessário **gerenciamento da estação** — controle de acesso, registro de sessões, supervisão remota — o que reforça a exigência de interfaces de comunicação adequadas.

### Comunicação prévia à distribuidora

| Artigo | Disposição |
|--------|------------|
| **Art. 550** | A instalação de estação de recarga deve ser **comunicada previamente à distribuidora** quando houver necessidade de: **(I)** conexão nova; **(II)** aumento ou redução de carga; ou **(III)** alteração do nível de tensão. |
| **Art. 551** | Os custos de adequação da rede e do sistema de medição seguem os critérios gerais da Resolução. |
| **Art. 553** | Na unidade consumidora com estação de recarga devem ser observadas as **normas e padrões da distribuidora** e as normas dos órgãos oficiais competentes (ABNT, NR-10 etc.). |

A comunicação é feita pelos canais da distribuidora (solicitação de conexão nova, aumento de carga ou alteração de tensão). Cada concessionária detalha o procedimento em norma técnica própria (ex.: Celesc, CPFL, Enel, Light). Instalações que **não alteram** a conexão existente podem não exigir novo processo — mas a conformidade com as normas técnicas locais permanece obrigatória.

### Protocolos abertos de comunicação

| Artigo | Disposição |
|--------|------------|
| **Art. 552** | Equipamentos de recarga que **não sejam exclusivos para uso privado** devem ser compatíveis com **protocolos abertos de domínio público** para: **(I)** comunicação; e **(II)** supervisão e controle remotos. |

**Uso privado exclusivo** — carregador restrito ao titular da unidade consumidora, sem cobrança ou acesso a terceiros — **não** está sujeito a essa exigência.

**Uso não exclusivo** — condomínios, empresas, locais com cobrança ou acesso compartilhado — exige protocolo aberto que permita a uma plataforma de terceiros monitorar e comandar o equipamento remotamente, sem dependência de software proprietário fechado.

Na prática de mercado, os protocolos mais utilizados para cumprir esse requisito são:

| Protocolo | Papel típico |
|-----------|--------------|
| **OCPP** (Open Charge Point Protocol) | Padrão de fato para gestão de redes de recarga: autenticação, início/parada de sessão, telemetria, firmware, cobrança. Versões comuns: 1.6 JSON e 2.0.1. |
| **Modbus TCP/RTU** | Supervisão e controle em integrações locais (EMS, automação predial, inversores fotovoltaicos). Protocolo aberto e amplamente documentado. |

A RN 1.000 **não nomeia** um protocolo específico; exige compatibilidade com padrões abertos. A escolha deve ser validada conforme o caso de uso e as normas técnicas da distribuidora local.

### Outras restrições relevantes

- **Art. 555**: vedada a injeção de energia na rede de distribuição a partir de veículos elétricos (V2G), salvo fluxo bidirecional restrito à mesma unidade consumidora.
- **Art. 556**: a distribuidora deve ressarcir danos elétricos em veículo elétrico, nas condições da Resolução.

### Referências

- [ANEEL — Veículos Elétricos](https://www.gov.br/aneel/pt-br/assuntos/veiculos-eletricos)
- [RN ANEEL nº 1.000/2021 (texto integral — DOU)](https://in.gov.br/web/dou/-/resolucao-normativa-aneel-n-1.000-de-7-de-dezembro-de-2021-*-375499427)

---

## Carregador GoodWe HCA G2

Wallbox AC da GoodWe (série G2), disponível em **7 kW** (monofásico), **11 kW** e **22 kW** (trifásico). Modelos de referência: `GW7K-HCA-20`, `GW11K-HCA-20`, `GW22K-HCA-20`.

| Característica | Especificação |
|----------------|---------------|
| Conector | Tipo 2 (cabo fixo ~7,5 m) |
| Proteção | IP66 / IP55 |
| DR integrado | 6 mA CC + 30 mA CA |
| Modo de recarga | Modo 3 (IEC 61851) |
| Protocolo de integração | **Modbus TCP** (rede); **Modbus RTU** (RS-485) |
| Plataforma nativa | **SEMS+** (app e portal GoodWe) |
| Métodos de partida | App, RFID, AUTO Start |

### Visão geral das interfaces

```
┌─────────────────────────────────────────────────────────────┐
│                    GoodWe HCA G2                            │
│                                                             │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌──────────────┐   │
│  │ RS-485  │  │ RS-485  │  │   LAN   │  │   Wi-Fi      │   │
│  │  (×2)   │  │  (×2)   │  │Ethernet │  │              │   │
│  └────┬────┘  └────┬────┘  └────┬────┘  └──────┬───────┘   │
│       │            │            │               │           │
│       └────────────┴──── Modbus TCP/RTU ────────┘           │
│                         │                                   │
│  ┌──────────┐    ┌──────┴──────┐    ┌──────────────────┐   │
│  │Bluetooth │    │  Plataforma │    │      RFID        │   │
│  │ (local)  │    │  (SEMS+ /   │    │  (acesso local)  │   │
│  └──────────┘    │   terceiros)│    └──────────────────┘   │
│                  └─────────────┘                              │
└─────────────────────────────────────────────────────────────┘
```

---

### RS-485 (2 portas)

**O que permite**

- Comunicação **Modbus RTU** com equipamentos de terceiros em barramento serial.
- Integração **cabeada** com inversores e baterias GoodWe para recarga inteligente (excedente solar, balanceamento de carga, troca de fase monofásico/trifásico).
- Conexão a **EMS** (Energy Management System) ou gateway local que traduza Modbus para a plataforma em nuvem.
- Em alguns mercados, conformidade com regulações de controle de carga (ex.: § 14a EnWG na Alemanha via Modbus TCP com EMS externo).

**Uso pela plataforma**

| Cenário | Aplicação |
|---------|-----------|
| Integração PV+EV | Ligar o carregador ao inversor/bateria GoodWe via RS-485 para modos de recarga com excedente fotovoltaico e limitação dinâmica de potência. |
| EMS local | Gateway ou controlador lê/escreve registradores Modbus (status, corrente, energia, comandos start/stop). |
| Automação predial | PLC ou concentrador industrial coleta telemetria e envia à plataforma via protocolo próprio. |

Parâmetros típicos do barramento (conforme documentação GoodWe para família de inversores): baud rate padrão **9600**, endereço Modbus configurável. O mapa de registradores específico do HCA G2 deve ser solicitado à GoodWe ou obtido no manual técnico do equipamento.

---

### LAN (Ethernet)

**O que permite**

- Conexão de rede **cabeada e estável** à LAN local.
- Comunicação **Modbus TCP** (porta **502**, padrão da indústria) para supervisão e controle por sistemas de terceiros.
- Integração com EMS/SMGW externo sem depender de Wi-Fi.
- Base recomendada para operação contínua em ambientes comerciais e condomínios.

**Uso pela plataforma**

| Cenário | Aplicação |
|---------|-----------|
| Backend de gestão | Servidor na rede local ou em nuvem (via VPN) consulta registradores Modbus TCP: estado do carregador, potência, energia acumulada, falhas. |
| Controle remoto | Comandos de limitação de corrente, habilitação/desabilitação de recarga, agendamento via escrita em registradores. |
| Conformidade regulatória | Via Modbus TCP (protocolo aberto), atende o espírito do Art. 552 da RN 1.000 para uso não exclusivo, desde que a plataforma implemente supervisão e controle remotos. |

Requisito de rede: IP do carregador e do servidor/gateway na **mesma sub-rede** (ou roteamento configurado). O Modbus TCP deve estar habilitado nas configurações do equipamento (via SEMS+ ou interface local).

---

### Wi-Fi

**O que permite**

- Conexão do carregador à rede sem fio local e à **nuvem GoodWe (SEMS+)**.
- Monitoramento remoto, agendamento de recargas e configuração de modos de operação pelo app **SEMS+**.
- Alternativa ao cabo Ethernet quando a infraestrutura LAN não está disponível.
- Pode servir de meio para Modbus TCP quando o equipamento está na mesma rede IP que o integrador (dependendo do firmware e configuração).

**Uso pela plataforma**

| Cenário | Aplicação |
|---------|-----------|
| SEMS+ (nativo) | Proprietário ou instalador acompanha sessões, histórico de energia, relatórios de custo e modos de recarga solar/rede. |
| Integração terceiros | Possível via Modbus TCP na rede Wi-Fi local; para gestão em nuvem proprietária, avaliar gateway ou API SEMS+ conforme contrato GoodWe. |
| Comissionamento | Configuração inicial da rede, modos de carga e pareamento com inversor/bateria GoodWe. |

**Limitação**: modos avançados de recarga com excedente solar podem estar disponíveis **apenas no ecossistema GoodWe** (inversor + carregador + SEMS+).

---

### Bluetooth

**O que permite**

- **Acesso local** pelo app SEMS+ sem necessidade de internet.
- Comissionamento em campo: leitura de parâmetros, alteração de configurações, diagnóstico.
- Conexão direta smartphone ↔ carregador para setup inicial (senha padrão de acesso local: `1234`, alterável no primeiro login).

**Uso pela plataforma**

| Cenário | Aplicação |
|---------|-----------|
| Instalação | Técnico configura rede, RFID, limites de corrente e integração Modbus antes de colocar em produção. |
| Manutenção | Diagnóstico local sem depender da conectividade de rede. |
| Operação contínua | **Não** é o canal adequado para supervisão 24/7; o Bluetooth desliga após ~5 minutos sem conexão ativa (salvo opção "Bluetooth Stays On" no app). |

A plataforma de gestão em produção deve usar **LAN/Wi-Fi (Modbus TCP)** ou **RS-485 (Modbus RTU)**; Bluetooth é ferramenta de campo.

---

### RFID

**O que permite**

- **Controle de acesso** ao ponto de recarga: apenas cartões/tags autorizados iniciam a sessão.
- Identificação de usuário para **rateio de custos** e reembolso (útil em empresas, condomínios e frotas).
- Método de partida alternativo ao app (junto com AUTO Start e partida via SEMS+).
- O equipamento é fornecido com **2 cartões RFID**; é possível cadastrar múltiplos cartões no sistema.

**Uso pela plataforma**

| Cenário | Aplicação |
|---------|-----------|
| Uso compartilhado | Cada morador/funcionário recebe um cartão; sessões ficam associadas ao identificador RFID para faturamento ou relatório. |
| Exploração comercial | RFID como método de autenticação local; a plataforma correlaciona o `idTag` (ou equivalente interno) ao usuário e à cobrança. |
| Integração com OCPP | Em modelos com suporte a OCPP, o RFID pode ser reportado ao CSMS via mensagens `Authorize` / `StartTransaction` (verificar disponibilidade por modelo e região). |

O RFID opera **no dispositivo**; a plataforma obtém o vínculo usuário-sessão via Modbus, SEMS+ ou OCPP, conforme o canal de integração escolhido.

---

## API GoodWe (SEMS Portal / SEMS+) — dados do carregador

A GoodWe **não publica documentação aberta** desses endpoints no portal. Eles são usados internamente pelo app/portal SEMS+ e foram identificados via inspeção de tráfego de rede (DevTools / HAR). Podem mudar sem aviso; para integração em produção, solicitar a **OpenAPI oficial** à GoodWe.

**Host observado (SEMS+):** `eu-gateway.semsportal.com`  
**Autenticação:** sessão/token do portal (mesmo login do SEMS+); requisições autenticadas como no app web.

### Endpoints observados

| Endpoint | Caminho (relativo) | Finalidade |
|----------|-------------------|------------|
| **getLastCharge** | `/web/sems/sems-plant/api/v1/chargePile/getLastCharge` | Última sessão / registo de carregamento (histórico recente) |
| **detail** | `/web/sems/sems-remote/api/ev-charger/detail` | Estado atual do carregador, agendamento e limites |

Outros endpoints reportados pela comunidade (não validados neste projeto):

- `/web/sems/sems-plant/api/equipments/information?deviceType=EV_CHARGER` — telemetria do equipamento
- `/web/sems/sems-plant/api/web/device/station/getDevicesByType?deviceType=EV_CHARGER` — lista de carregadores da usina

### Resposta padrão

Todas as respostas seguem o envelope:

| Campo | Descrição |
|-------|-----------|
| `code` | `"00000"` = sucesso |
| `description` | Mensagem (ex.: `"成功"`) |
| `traceId` | ID de rastreamento da requisição |
| `data` | Payload específico do endpoint |

### `getLastCharge` — sessão e energia entregue

Corresponde ao **Registo de carregamento** no portal (duração, kWh, autonomia estimada, cartão RFID, porta).

**Exemplo de resposta (`data.chargeLog`):**

```json
{
  "chargePileSN": "57000HPA247L0002",
  "currentChargeQuantity": 14.6,
  "chargeTimeLength": 355,
  "mileage": 73,
  "unit": "km",
  "chargeCardNumber": "57000HPA247L0002",
  "charGun": 1,
  "averCharP": 2.7630985915492956,
  "maxCharP": 3.5,
  "status": 0,
  "overViewStatus": 1,
  "chartUnit": "kWh",
  "workStu": 8
}
```

| Campo | Tipo | Significado provável | Exemplo / UI |
|-------|------|----------------------|--------------|
| `chargePileSN` | string | Serial number do carregador | `57000HPA247L0002` |
| `currentChargeQuantity` | number | Energia entregue na sessão | **14,60 kWh** |
| `chargeTimeLength` | number | Duração da sessão (**minutos**) | 355 → **5 h 55 min** |
| `mileage` | number | Autonomia estimada adicionada | **73 km** (`unit`: `"km"`) |
| `chargeCardNumber` | string | ID do cartão RFID usado | **ID do cartão** no portal |
| `charGun` | number | Porta / conector de recarga | **1** → porta de carregamento 1 |
| `averCharP` | number | Potência média durante a sessão | ~2,76 kW |
| `maxCharP` | number | Potência máxima atingida | 3,5 kW |
| `status` | number | Código interno de status da sessão | 0 |
| `overViewStatus` | number | Status resumido para overview | 1 |
| `chartUnit` | string | Unidade do gráfico de energia | `"kWh"` |
| `workStu` | number | Estado operacional (código numérico) | 8 |

**Eventos de sessão:** este endpoint expõe a **última sessão registrada** (ou sessão em destaque no histórico). Para listar múltiplas sessões no intervalo de datas do portal, é provável que exista outro endpoint acionado ao filtrar por período — mapear via HAR ao navegar na tela *Registo de carregamento*.

### `detail` — status em tempo real e agendamento

Estado **atual** do carregador: disponibilidade, modo, agendamento e limites de potência/energia.

**Exemplo de resposta (`data`):**

```json
{
  "chargeMaxPower": 7,
  "chargedNow": 170,
  "currentLimit": 0,
  "dynamicLoad": 0,
  "ensureMinimumChargingPower": 0,
  "finishTime": "0",
  "lockChargingPlug": 0,
  "maxEnergy": 0,
  "minEnergy": 0,
  "phaseSwitch": 170,
  "soc": 0,
  "scheduleChargeMaxPower": 0,
  "scheduleFinishTime": "0",
  "scheduleMaxEnergy": 100,
  "scheduleMinEnergy": 100,
  "scheduleSOC": 99,
  "chargeFromGrid": 0,
  "chargeMode": 0,
  "scheduleChargeMode": 2,
  "scheduleMode": 0,
  "scheduleTime": "2026-06-20 02:00:00",
  "scheduleTotalMinute": 60,
  "status": "available",
  "workState": "available_gun_no_insered",
  "startStatus": false,
  "unit": 0,
  "mileage": 5
}
```

#### Status e estado operacional

| Campo | Tipo | Significado provável |
|-------|------|----------------------|
| `status` | string | Estado de alto nível | `"available"` = disponível |
| `workState` | string | Detalhe do estado | `"available_gun_no_insered"` = disponível, pistola não conectada |
| `startStatus` | boolean | Recarga em andamento | `false` = não está carregando |
| `workStu` (em `getLastCharge`) | number | Código numérico paralelo ao `workState` | Verificar tabela de enums no firmware |

#### Potência e limites

| Campo | Significado provável |
|-------|----------------------|
| `chargeMaxPower` | Potência máxima configurada (kW) — ex.: **7** (modelo 7 kW) |
| `currentLimit` | Limite de corrente (0 = sem limite ativo ou padrão) |
| `dynamicLoad` | Balanceamento dinâmico de carga habilitado/configurado |
| `ensureMinimumChargingPower` | Garantia de potência mínima de recarga |
| `phaseSwitch` | Troca de fase (monofásico/trifásico) — valor codificado (ex.: 170) |
| `chargedNow` | Energia ou progresso da sessão atual (codificado; unidade via `unit`) |

#### Modos e agendamento

| Campo | Significado provável |
|-------|----------------------|
| `chargeMode` | Modo de recarga ativo (solar, rede, híbrido etc.) |
| `scheduleChargeMode` | Modo do agendamento — ex.: **2** |
| `scheduleMode` | Tipo de agendamento |
| `scheduleTime` | Início agendado | `"2026-06-20 02:00:00"` |
| `scheduleTotalMinute` | Duração agendada (minutos) | **60** |
| `scheduleFinishTime` | Fim agendado (`"0"` = não definido) |
| `scheduleMaxEnergy` / `scheduleMinEnergy` | Limites de energia do agendamento (%) |
| `scheduleSOC` | SOC alvo do agendamento (%) — ex.: **99** |
| `chargeFromGrid` | Permite/com indica recarga da rede |

#### Outros

| Campo | Significado provável |
|-------|----------------------|
| `soc` | State of Charge do veículo (0 se não comunicado pelo EV) |
| `mileage` | Autonomia estimada associada ao estado atual |
| `lockChargingPlug` | Bloqueio da pistola |
| `maxEnergy` / `minEnergy` | Limites de energia da sessão |
| `finishTime` | Horário de término (`"0"` = N/A) |
| `unit` | Unidade interna para campos numéricos codificados |

### Síntese: o que a API expõe sobre o carregador

| Categoria | Endpoint | Dados disponíveis |
|-----------|----------|-------------------|
| **Status** | `detail` | `status`, `workState`, `startStatus`, códigos `workStu` |
| **Potência** | `detail`, `getLastCharge` | `chargeMaxPower`, `currentLimit`, `averCharP`, `maxCharP`, `dynamicLoad` |
| **Energia entregue** | `getLastCharge` | `currentChargeQuantity` (kWh), `chartUnit` |
| **Sessão / eventos** | `getLastCharge` | duração (`chargeTimeLength`), cartão RFID (`chargeCardNumber`), porta (`charGun`), SN (`chargePileSN`) |
| **Agendamento** | `detail` | `scheduleTime`, `scheduleTotalMinute`, modos e limites SOC/energia |
| **Estimativas** | Ambos | `mileage` (km de autonomia) |

> **Limitação:** `getLastCharge` retorna predominantemente a **última sessão**; o histórico completo visível no portal (várias sessões por intervalo de datas) provavelmente usa endpoint(s) adicional(is) ainda não mapeado(s) neste documento.

> **Produção:** preferir **Modbus TCP** local ou **OpenAPI oficial** (com NDA) em vez de depender dos endpoints internos do SEMS+.

---

## Integração com plataforma — resumo prático

| Objetivo | Interface recomendada | Protocolo |
|----------|----------------------|-----------|
| Supervisão e controle remoto (produção) | LAN ou Wi-Fi | Modbus TCP |
| Integração com inversor/EMS local | RS-485 | Modbus RTU |
| Comissionamento e suporte em campo | Bluetooth + app SEMS+ | — |
| Gestão nativa GoodWe (residencial/PV+EV) | Wi-Fi | SEMS+ (nuvem) |
| Controle de acesso no ponto de carga | RFID | Local + correlacionamento na plataforma |
| Rede de recarga comercial / multi-ponto | LAN (+ OCPP se disponível no modelo) | Modbus TCP e/ou OCPP |

### Adequação à RN ANEEL 1.000/2021

| Situação | Requisito Art. 552 | Caminho no HCA G2 |
|----------|-------------------|-------------------|
| Uso **exclusivo privado** (apenas o titular da UC) | Não aplicável | SEMS+ ou operação local suficientes |
| Uso **não exclusivo** (terceiros, cobrança, condomínio) | Protocolo aberto para comunicação e supervisão/controle remotos | **Modbus TCP** via LAN/Wi-Fi; eventualmente **OCPP** em modelos/versões que o suportem (confirmar com GoodWe para o mercado brasileiro) |

> **Nota**: o suporte a **OCPP** é citado para alguns modelos/mercados (ex.: Austrália), mas não consta de forma explícita na documentação EMEA global. Para projetos de exploração comercial no Brasil, validar com o distribuidor GoodWe local a versão de firmware, o mapa Modbus do HCA G2 e a disponibilidade de OCPP antes do dimensionamento da plataforma.

### Documentação técnica GoodWe

- [HCA G2 Series — GoodWe EMEA](https://emea.goodwe.com/ie/hca-g2)
- Manual de operação, datasheet e mapa Modbus: solicitar ao suporte GoodWe Brasil ou ao instalador credenciado.

---

## Mapeamento de APIs complementares

APIs externas que podem **enriquecer a plataforma** além dos dados nativos do carregador GoodWe (SEMS+/Modbus). Nenhuma substitui a telemetria operacional do HCA G2; cada uma cobre uma camada diferente: descoberta de pontos públicos, contexto geográfico, regulação e indicadores do setor elétrico.

| API | Camada | Autenticação | Custo |
|-----|--------|--------------|-------|
| [Open Charge Map](#open-charge-map-api) | Mapa global de estações de recarga | API Key gratuita | Gratuita |
| [Google Places](#google-places-api-evchargeoptions) | Detalhes de postos EV (conectores, potência) | API Key Google Cloud | Paga (SKU Enterprise+) |
| [ANEEL Open Data](#aneel-open-data) | Regulação, distribuidoras, indicadores setoriais | Não (dados abertos) | Gratuita |
| [IBGE Localidades](#ibge-api-de-localidades) | Divisão administrativa oficial do Brasil | Não | Gratuita |

---

### Open Charge Map API

Maior registro **open data** mundial de locais de recarga de VE. Útil para comparar a instalação privada (GoodWe) com a rede pública ao redor, exibir mapas e validar cobertura regional.

**Documentação:** [openchargemap.org/develop/api](https://openchargemap.org/develop/api)  
**Base URL:** `https://api.openchargemap.io/v3`

#### Autenticação

- Registrar em [openchargemap.org/site/register](https://openchargemap.org/site/register) e obter **API Key**.
- Enviar a chave como parâmetro `key=` na URL ou header `X-API-Key`.
- Recomendado: header `User-Agent` identificando a aplicação.

#### Endpoints principais

| Endpoint | Método | Descrição |
|----------|--------|-----------|
| `/poi/` | GET | Lista de POIs (estações) com filtros geográficos |
| `/referencedata/` | GET | Tabelas auxiliares: tipos de conector, operadores, países, status |

#### Parâmetros úteis (`/poi/`)

| Parâmetro | Exemplo | Uso |
|-----------|---------|-----|
| `countrycode` | `BR` | Filtrar por país (ISO) |
| `latitude`, `longitude`, `distance` | `-23.55`, `-46.62`, `10` | Raio em km/milhas a partir de um ponto |
| `boundingbox` | `lat_min,lon_min,lat_max,lon_max` | Retângulo geográfico |
| `connectiontypeid` | `25` | Tipo de conector (ex.: Type 2) |
| `levelid` | `2` | Nível de recarga (1/2/3) |
| `operatorid` | — | Rede operadora |
| `statustypeid` | — | Operacional / planejado / indisponível |
| `maxresults` | `100` | Limite de resultados (padrão 100) |
| `compact`, `verbose` | `true`/`false` | Controlar tamanho da resposta |

#### Dados expostos por POI

| Objeto / campo | Conteúdo |
|----------------|----------|
| `AddressInfo` | Endereço, latitude, longitude, título do local |
| `Connections[]` | Conectores: tipo, potência (kW), quantidade, nível |
| `OperatorInfo` | Operador / rede de recarga |
| `UsageType` | Público, privado, membros etc. |
| `StatusType` | Status operacional do ponto |
| `DateLastStatusUpdate` | Última atualização de status |
| `UserComments` | Check-ins e comentários da comunidade (se `includecomments=true`) |

#### Exemplo de chamada (São Paulo, raio 5 km)

```http
GET https://api.openchargemap.io/v3/poi/?output=json&countrycode=BR&latitude=-23.5505&longitude=-46.6333&distance=5&distanceunit=KM&maxresults=20&compact=true&verbose=false&key=SUA_API_KEY
```

#### Uso pela plataforma

| Cenário | Aplicação |
|---------|-----------|
| Mapa de cobertura | Exibir estações públicas próximas à usina LAB FIAP Eco Smart Home |
| Benchmark | Comparar potência/conectores do HCA G2 com pontos OCM na região |
| Planejamento comercial | Identificar lacunas de recarga pública no entorno (exploração Art. 554 RN 1.000) |

> **Limitação:** cobertura no Brasil é **desigual** e depende de contribuições da comunidade; dados podem estar desatualizados. Validar com fonte primária quando possível.

---

### Google Places API (`evChargeOptions`)

API comercial do Google Maps para obter **informações estruturadas de recarga** em locais catalogados (postos, shoppings, estacionamentos). Complementa o OCM com dados do ecossistema Google, incluindo agregação por tipo de conector e, quando disponível, disponibilidade em tempo real.

**Documentação:** [Places API (New) — Place resource](https://developers.google.com/maps/documentation/places/web-service/reference/rest/v1/places)  
**Campo:** `evChargeOptions` (SKU **Place Details Enterprise + Atmosphere**)

#### Como obter `evChargeOptions`

1. **Nearby Search (New)** ou **Text Search (New)** — buscar lugares do tipo estação de recarga.
2. **Place Details (New)** — detalhar o `place_id` com FieldMask incluindo `evChargeOptions`.

```http
GET https://places.googleapis.com/v1/places/{PLACE_ID}
Headers:
  X-Goog-Api-Key: SUA_API_KEY
  X-Goog-FieldMask: id,displayName,formattedAddress,location,evChargeOptions
```

#### Estrutura de `evChargeOptions`

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `connectorCount` | integer | Número total de conectores na estação |
| `connectorAggregation[]` | array | Agrupamento por tipo e potência |
| `connectorAggregation[].type` | enum | Ex.: `EV_CONNECTOR_TYPE_CCS_DC_2`, `EV_CONNECTOR_TYPE_TYPE_2` |
| `connectorAggregation[].maxChargeRateKw` | number | Potência máxima (kW) do grupo |
| `connectorAggregation[].count` | integer | Quantidade de conectores desse tipo/potência |
| `connectorAggregation[].availableCount` | integer | Conectores disponíveis (quando informado) |
| `connectorAggregation[].outOfServiceCount` | integer | Conectores fora de serviço |
| `connectorAggregation[].availabilityLastUpdateTime` | timestamp | Última atualização de disponibilidade |

Campo relacionado: `evChargeAmenitySummary` — resumo de amenidades próximas à estação (Enterprise + Atmosphere).

#### Exemplo simplificado de resposta

```json
{
  "evChargeOptions": {
    "connectorCount": 4,
    "connectorAggregation": [
      {
        "type": "EV_CONNECTOR_TYPE_TYPE_2",
        "maxChargeRateKw": 22.0,
        "count": 2,
        "availableCount": 1,
        "outOfServiceCount": 0
      }
    ]
  }
}
```

#### Uso pela plataforma

| Cenário | Aplicação |
|---------|-----------|
| Enriquecimento de POIs | Complementar endereço, horário e conectores de locais públicos |
| Disponibilidade | Exibir conectores livres/ocupados quando Google fornece `availableCount` |
| UX do morador | Mostrar opções de recarga externa quando o carregador condominial estiver ocupado |

> **Limitação:** API **paga** (billing por SKU); exige projeto Google Cloud e chave com Places API habilitada. Cobertura depende do cadastro Google Maps — nem todo carregador privado (ex.: HCA G2 em condomínio) aparecerá.

---

### ANEEL Open Data

Portal oficial de **dados abertos** da Agência Nacional de Energia Elétrica. Não há, até o momento, conjunto dedicado e consolidado de **cadastro nacional de estações de recarga**; a ANEEL trata o tema principalmente via **RN 1.000/2021** (Cap. V) e agenda regulatória em evolução (ex.: CP 42/2025 sobre conexão de carregadores).

**Portal:** [dadosabertos.aneel.gov.br](https://dadosabertos.aneel.gov.br/)  
**Contexto regulatório VE:** [gov.br/aneel — Veículos Elétricos](https://www.gov.br/aneel/pt-br/assuntos/veiculos-eletricos)

#### APIs disponíveis

| API | Base URL | Padrão |
|-----|----------|--------|
| **CKAN** (catálogo de datasets) | `https://dadosabertos.aneel.gov.br/api/3/action/` | REST JSON |
| **Search API** (ArcGIS / OGC Records) | `https://dadosabertos-aneel.opendata.arcgis.com/api/search/v1/` | OGC API — Records |

#### CKAN — endpoints úteis

| Action | Exemplo | Uso |
|--------|---------|-----|
| `package_search` | `.../package_search?q=distribuidora&rows=10` | Buscar conjuntos de dados por palavra-chave |
| `package_show` | `.../package_show?id={id}` | Metadados e recursos de um dataset |
| `resource_show` | `.../resource_show?id={id}` | URL de download de CSV/JSON/XLSX |
| `datastore_search` | `.../datastore_search?resource_id={id}` | Consultar registros tabulares via API |

#### Conjuntos relevantes para a plataforma

| Conjunto | Utilidade |
|----------|-----------|
| **INDGER** (Indicadores Gerenciais da Distribuição) | Qualidade comercial/técnica das distribuidoras — contexto para SLA e comunicação com concessionária |
| Dados de **concessionárias e permissionárias** | Área de concessão, contatos, normas técnicas locais |
| Indicadores de **continuidade** (DEC/FEC) | Correlacionar disponibilidade de energia com operação de carregadores |
| Projetos e **tarifas** | Análise de custo de energia para precificação de recarga comercial |

#### Exemplo de busca CKAN

```http
GET https://dadosabertos.aneel.gov.br/api/3/action/package_search?q=indger&rows=5
```

#### Search API (OGC) — exemplo

```http
GET https://dadosabertos-aneel.opendata.arcgis.com/api/search/v1/collections
GET https://dadosabertos-aneel.opendata.arcgis.com/api/search/v1/collections/{collectionId}/items
```

Documentação interativa: [dadosabertos-aneel.opendata.arcgis.com/api/search/definition/](https://dadosabertos-aneel.opendata.arcgis.com/api/search/definition/)

#### Uso pela plataforma

| Cenário | Aplicação |
|---------|-----------|
| Compliance | Referenciar base legal (RN 1.000) e normas da distribuidora da área de concessão |
| Due diligence | Verificar indicadores da distribuidora antes de solicitar aumento de carga (Art. 550) |
| Precificação | Cruzar tarifas e bandeiras com custo de energia das sessões de recarga |
| Expansão | Monitorar futuros datasets de infraestrutura de recarga (agenda ANEEL 2025+) |

> **Limitação:** dados de **localização/potência de carregadores instalados** ainda não estão centralizados na ANEEL; usar OCM/Google para mapa e SEMS+/Modbus para dados operacionais próprios.

---

### IBGE — API de Localidades

Serviço público **gratuito e sem autenticação** do IBGE com a divisão político-administrativa oficial do Brasil. Essencial para normalizar endereços, agrupar métricas por município/UF/região e cruzar dados da plataforma com estatísticas demográficas.

**Documentação:** [servicodados.ibge.gov.br/api/docs/localidades](https://servicodados.ibge.gov.br/api/docs/localidades)  
**Base URL:** `https://servicodados.ibge.gov.br/api/v1/localidades`

#### Endpoints principais

| Endpoint | Retorno |
|----------|---------|
| `GET /estados` | 27 UFs com id, sigla, nome, região |
| `GET /estados/{UF}/municipios` | Municípios de uma UF |
| `GET /municipios` | Todos os municípios (`?orderBy=nome`) |
| `GET /municipios/{id}` | Detalhe de um município (id IBGE, ex.: `3550308` = São Paulo) |
| `GET /regioes` | Regiões (N, NE, CO, SE, S) |
| `GET /regioes-imediatas`, `/regioes-intermediarias` | Divisões geográficas IBGE (substituem meso/microprogressivamente) |

#### Exemplo — municípios de São Paulo

```http
GET https://servicodados.ibge.gov.br/api/v1/localidades/estados/SP/municipios?orderBy=nome
```

Resposta (campos típicos por município): `id`, `nome`, `microrregiao`, `regiao-imediata`, `regiao-intermediaria`, UF e região.

#### Uso pela plataforma

| Cenário | Aplicação |
|---------|-----------|
| Geocodificação administrativa | Resolver "Cambuci, São Paulo" → código IBGE `3550308` para dashboards |
| Segmentação | Agrupar sessões de recarga e geração solar por município/UF |
| Integração ANEEL/OCM | Padronizar filtros geográficos entre APIs com códigos oficiais |
| Relatórios | Enriquecer exportações (planilhas SEMS) com região e mesorregião IBGE |

> **Dados demográficos adicionais** (população, PIB etc.) estão em outras APIs IBGE (`/api/agregados/`, SIDRA); a API de Localidades cobre apenas a estrutura territorial.

---

### Síntese: enriquecimento da plataforma GoodWe

```
                    ┌─────────────────────────────────────┐
                    │         Plataforma GoodWe-01        │
                    │  (SEMS+ / Modbus / sessões / RFID)  │
                    └──────────────┬──────────────────────┘
                                   │
         ┌─────────────────────────┼─────────────────────────┐
         │                         │                         │
         ▼                         ▼                         ▼
┌─────────────────┐    ┌─────────────────────┐    ┌─────────────────┐
│ Open Charge Map │    │  Google Places API  │    │  IBGE           │
│ Mapa público EV │    │  evChargeOptions    │    │  Municípios/UF  │
│ Cobertura BR    │    │  Conectores/potência│    │  Segmentação    │
└─────────────────┘    └─────────────────────┘    └─────────────────┘
                                   │
                                   ▼
                        ┌─────────────────────┐
                        │  ANEEL Open Data    │
                        │  Regulação, INDGER, │
                        │  distribuidoras     │
                        └─────────────────────┘
```

| Necessidade da plataforma | API recomendada |
|---------------------------|-----------------|
| Telemetria do **seu** carregador HCA G2 | SEMS+ / Modbus (documentado acima) |
| Mapa de recarga **pública** ao redor | Open Charge Map + Google Places |
| Normalizar **localização** no Brasil | IBGE Localidades |
| Contexto **regulatório e tarifário** | ANEEL Open Data + RN 1.000 (Cap. V) |
| Disponibilidade de conectores em tempo real | Google Places (`availableCount`) |

---

## Referências adicionais

- ABNT NBR IEC 61851-1 — Sistemas de recarga de veículos elétricos (Modos 1 a 4).
- Open Charge Alliance — [OCPP](https://www.openchargealliance.org/) (protocolo aberto para gestão de redes de recarga).
- Modbus Organization — [Modbus TCP/RTU](https://modbus.org/) (protocolo aberto para supervisão industrial).
- Open Charge Map — [API Documentation](https://openchargemap.org/develop/api)
- Google Places API — [Place Details (New)](https://developers.google.com/maps/documentation/places/web-service/place-details) · [`evChargeOptions`](https://developers.google.com/maps/documentation/places/web-service/reference/rest/v1/places#EVChargeOptions)
- ANEEL — [Portal de Dados Abertos](https://dadosabertos.aneel.gov.br/) · [Search API (OGC)](https://dadosabertos-aneel.opendata.arcgis.com/api/search/definition/)
- IBGE — [API de Localidades](https://servicodados.ibge.gov.br/api/docs/localidades)
