# Classificador de Pedidos e-SIC (Público vs Não Público) — Hackathon CGDF

Solução para identificar automaticamente pedidos de acesso à informação previamente marcados como públicos que contêm dados pessoais (ex.: nome, CPF, RG, telefone, e-mail) e que, portanto, devem ser classificados como "contendo dados pessoais", em conformidade com a LGPD, com a LAI e os requisitos do edital.

---

## Objetivos da solução

- Detectar dados pessoais em pedidos de acesso à informação (identificação direta ou indireta de pessoa natural).
- Classificar cada pedido como:
  - PÚBLICO: não contém dados pessoais;
  - contendo dados pessoais: contém dados pessoais.
- Atualizar automaticamente a planilha de entrada, escrevendo a decisão na coluna C.
- Utilizar abordagem híbrida:
  - Regras determinísticas (expressões regulares) para padrões fortes (CPF, e-mail, telefone etc.).
  - Modelo de linguagem (ChatGPT via API OpenAI) para casos semânticos mais complexos.

---

## Estrutura do projeto

```
.
├── main.py
├── requirements.txt
└── README.md
└── dados
  └── AMOSTRA_e-SIC.xlsx
  └── AMOSTRA_e-SIC_classificada.xlsx
  
```

### Função de cada arquivo

- main.py  
  Script principal. Lê a planilha XLSX, classifica cada pedido e grava o resultado na coluna C. Executado via linha de comando.

- requirements.txt  
  Lista de dependências Python necessárias para o projeto.

- README.md  
  Documentação de pré-requisitos, configuração e execução.

- dados/AMOSTRA_e-SIC.xlsx  
  Dados sintéticos gerados pela CGDF que devem ser utilizados para avaliação do modelo.

- dados/AMOSTRA_e-SIC_classificada.xlsx  
  Arquivo contendo resultado da análise após aplicação do modelo nos dados contidos no arquivo AMOSTRA_e-SIC.xlsx.

---

## Pré-requisitos

- Python 3.9 ou superior (recomendado 3.10+)
- Conexão com a internet
- Arquivo XLSX de entrada com os pedidos do e-SIC

---

## Configuração do ambiente

### 1. Criar e ativar ambiente virtual

#### Windows (PowerShell)
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

#### macOS / Linux
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Atualizar o pip

```bash
python -m pip install --upgrade pip
```

### 3. Instalar dependências

```bash
pip install -r requirements.txt
```

---

## Configuração da variável de ambiente para conexão com a IA

Alterar valor da variável "api_key" na linha 64 do arquivo main.py

---

## Execução do código

Exemplo de execução completa:

```bash
python main.py \
  --input "dados/AMOSTRA_e-SIC.xlsx" \
  --output "dados/AMOSTRA_e-SIC_classificada.xlsx" \
  --sheet "Amostra - SIC" \
  --model "gpt-4.1-mini" \
  --col_text 2 \
  --col_out 3
```

Exemplo de execução simplificada, onde o script buscará os dados na pasta padrão, isto é, dentro da pasta dados. Ele também irá buscar o arquivo com nome padrão, isto é, AMOSTRA_e-SIX.xlsx:

```bash
python main.py 
```

### Argumentos disponíveis

- --input: caminho do arquivo XLSX de entrada  
- --output: caminho do arquivo XLSX de saída  
- --sheet: nome da aba da planilha  
- --model: modelo da OpenAI a ser utilizado  
- --col_text: coluna do texto do pedido (1=A, 2=B, ...)  
- --col_out: coluna onde será escrita a classificação  

---

## Entrada

- Arquivo XLSX a ser inserido na pasta data, preferencialmente com o nome AMOSTRA_e-SIC.xlsx
  - É esperado que os dados a serem analisados estejam contidos na coluna B da aba "Amostra - SIC", dentro do arquivo AMOSTRA_e-SIC.xlsx, mas isso pode ser alterado nos argumentos disponíveis citados acima.

---

## Saída

- Arquivo XLSX classificado
- Coluna C preenchida com dois valores possíveis:
  - PÚBLICO; ou
  - contendo dados pessoais.

---

## Observações finais

- A solução prioriza segurança e conformidade com a LGPD e com a LAI.
- Os dados utilizados para teste podem conter informações sintéticas, conforme previsto no edital.
- O pipeline é auditável e pode ser facilmente integrado a fluxos de revisão humana.

Projeto desenvolvido para o Hackathon da CGDF — categoria Acesso à Informação.
