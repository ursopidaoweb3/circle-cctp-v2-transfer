# 🚀 Circle CCTP V2 - Transferência Nativa USDC

Transferência automática de USDC de **ETH Sepolia** para **Arc Testnet** em apenas **~2 minutos**!

Implementação oficial do protocolo **CCTP V2** da Circle com SDK Developer-Controlled Wallets.

## ✨ Features

- ✅ **100% Automático** - 4 etapas em 1 comando
- ✅ **Ultra-rápido** - Attestação em 15 segundos  
- ✅ **Links Diretos** - TX URLs clicáveis para cada etapa
- ✅ **V2 Official** - Usa endereços testnet oficiais da Circle
- ✅ **Flexível** - Suporta qualquer quantidade de USDC

## 🚀 Quick Start

### 1. Instalar Dependências
```bash
pip install -r requirements.txt
```

### 2. Configurar Variáveis de Ambiente

Crie um arquivo `.env`:

```bash
cp .env.example .env
```

Edite com suas chaves da Circle API (testnet):

```env
CIRCLE_API_KEY=seu_testnet_api_key
CIRCLE_ENTITY_SECRET=seu_testnet_entity_secret
```

⚠️ **Nunca commite `.env` com credenciais reais!** (Já está em .gitignore)

### 3. Executar Transferência

```bash
python src/transfer.py
```

Aguarde ~2 minutos para conclusão automática das 4 etapas!

## 📋 Processo (4 Etapas Automáticas)

```
1️⃣  APPROVE      → TokenMessenger V2 autorizado (~30s)
                   Link: https://sepolia.etherscan.io/tx/...

2️⃣  BURN         → USDC queimado em Sepolia (~30s)
                   Link: https://sepolia.etherscan.io/tx/...

3️⃣  ATTESTATION  → Assinatura da Circle V2 (~15s)
                   Polling automático

4️⃣  MINT         → USDC recebido em Arc Testnet (~30s)
                   Link: https://sepolia-explorer.arbitrum.io/tx/...
```

## 🔧 Customização

### Mudar o Valor de USDC

Edite `src/transfer.py` linha 25:

```python
AMOUNT = "5000000"   # 5 USDC (em wei)
# 1 USDC = 1000000
# 10 USDC = 10000000
```

### Mudar o Destinatário

Edite `src/transfer.py` linha 15:

```python
DESTINATION_ADDRESS = "0xseu_endereco_aqui"
```

### Gerar Novas Carteiras (Opcional)

```bash
python src/create_wallet_eth_sepolia.py
python src/create_wallet_arc_testnet.py
```

### Verificar Saldo

```bash
python src/check_balance.py
```

### Financiar Carteiras

```bash
python src/fund_wallets.py
```

Ou obtenha fundos em:
- **ETH Sepolia**: https://sepolia-faucet.pk910.de
- **USDC Testnet**: https://faucet.circle.com

## 📊 Saída Esperada

```
======================================================================
🔄 CCTP V2 Transfer: ETH Sepolia → Arc Testnet
======================================================================
5 USDC → 0xb9a4522ced507631905e6540d7c9d4ea2d4ab973
======================================================================

1️⃣  Approve
✅

2️⃣  Burn
✅

3️⃣  Attestation
  ✅ Attestation received in 15s!

4️⃣  Mint
✅

======================================================================
✅ SUCCESS!
======================================================================

📋 TRANSACTION LINKS:

Approve USDC:
  0xc667bacbff0cac58cefc2b1113cc687acb692219a0f402940a84895c3adb1f8b
  https://sepolia.etherscan.io/tx/0xc667bacbff0cac58cefc2b1113cc687acb692219a0f402940a84895c3adb1f8b

Burn USDC:
  0x63c98183bc0aa972f745dc8f4bdcfe736e6b9d8e86520ea7563a9bc21d662f6f
  https://sepolia.etherscan.io/tx/0x63c98183bc0aa972f745dc8f4bdcfe736e6b9d8e86520ea7563a9bc21d662f6f

Mint USDC:
  0x81d62573546f7030650aeb0a920e3e3d1a60568845681990588b7c1a64b4a221
  https://sepolia-explorer.arbitrum.io/tx/0x81d62573546f7030650aeb0a920e3e3d1a60568845681990588b7c1a64b4a221

======================================================================
```

Todos os 3 TX hashes com links clicáveis são retornados automaticamente!

## 📚 Documentação Técnica

### V2 depositForBurn Signature (7 Parâmetros)

```python
depositForBurn(
    amount=5000000,                                              # Wei
    destinationDomain=26,                                        # Arc Testnet
    mintRecipient=0xb9a4522ced507631905e6540d7c9d4ea2d4ab973,  # Destinatário
    burnToken=0x1c7d4b196cb0c7b01d743fbc6116a902379c7238,      # USDC Sepolia
    destinationCaller=0x00...00,                                 # bytes32(0)
    maxFee=500,                                                  # 0.0005 USDC (obrigatório!)
    minFinalityThreshold=0                                       # Fast Transfer
)
```

### Attestation API V2

```
GET /v2/messages/{sourceDomain}?transactionHash={txHash}
```

Retorna:
```json
{
  "messages": [
    {
      "message": "0x...",
      "attestation": "0x...",
      "status": "complete"
    }
  ]
}
```

## 🔧 Solução de Problemas

| Erro | Solução |
|------|---------|
| "Import circle" error | `pip install circle-developer-controlled-wallets` |
| "Token USDC não encontrado" | Obter USDC em https://faucet.circle.com |
| "Unauthorized (401)" | Verificar CIRCLE_API_KEY e CIRCLE_ENTITY_SECRET |
| "Attestation timeout" | Verificar se maxFee=500 (não 0!) |
| "Saldo insuficiente" | Financiar carteira em testnet faucet |

## ⚙️ Configuração das Carteiras

```python
SOURCE_WALLET_ID = "2ed682d1-29d3-5398-a7d2-4c9f8f56f54d"        # Sepolia
DESTINATION_WALLET_ID = "12d2ca1e-d1a0-5d46-93e8-8d76c6aba102"  # Arc Testnet
DESTINATION_ADDRESS = "0xb9a4522ced507631905e6540d7c9d4ea2d4ab973"
```

## 📚 Recursos Adicionais

- [Circle CCTP Docs](https://developers.circle.com/cctp)
- [Circle V2 Smart Contracts](https://developers.circle.com/cctp/evm-smart-contracts)
- [Sepolia Etherscan](https://sepolia.etherscan.io)
- [Arc Testnet Explorer](https://sepolia-explorer.arbitrum.io)

## 🔐 Segurança

- ✅ Nunca commite credenciais reais
- ✅ Use `.env` para variáveis locais (ignorado pelo git)
- ✅ Use Secrets do Replit para produção
- ✅ Teste em testnet antes de mainnet

## 📝 Licença

MIT License - Veja LICENSE para detalhes
