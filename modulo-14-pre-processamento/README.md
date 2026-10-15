# Módulo 14 – Pré-processamento dos dados

[← Voltar ao projeto](../README.md)

Primeira etapa da pré-modelagem do projeto de churn: limpeza e tratamento da base original de clientes de telecomunicações (internet, telefone e TV).

**Autor:** Lucas Farias Epitácio de Almeida

## Base de dados

- `dados/CHURN_TELECON_MOD08_TAREFA.csv`: base original fornecida pelo curso (2.500 clientes, 16 colunas, separador `;`).
- `dados/data_preprocessed.csv`: base tratada gerada no final do notebook e usada no [Módulo 15](../modulo-15-eda/).

## O que foi feito

1. **Tipos de dados:** `Pagamento_Mensal` e `Total_Pago` estavam como texto e foram convertidas para número com `pd.to_numeric(errors='coerce')`.
2. **Dados faltantes:** levantamento por coluna e tratamento de cada caso com justificativa.
3. **Inconsistências de grafia:** padronização de valores escritos de formas diferentes.
4. **Renomeação:** colunas em inglês passaram para o português, no mesmo padrão das demais.
5. **Exportação:** a base tratada é salva em `dados/data_preprocessed.csv`.

## Tratamento dos dados faltantes

| Coluna | Faltantes | Tratamento | Motivo |
| --- | --- | --- | --- |
| `Churn` | 5 (0,20%) | Exclusão das linhas | É a variável-alvo; sem ela o registro não serve para o modelo |
| `Genero` | 12 (0,48%) | Moda (`Male`) | Variável categórica |
| `PhoneService` | 1.482 (59,28%) | Moda (`Yes`) | Excluir perderia mais da metade da base; `Yes` é 91,5% dos valores preenchidos |
| `Pagamento_Mensal` | 325 (13,00%) | Mediana (71,45) | Numérica; a mediana é mais robusta a valores extremos |

## Inconsistências corrigidas

- `Genero`: `F` e `f` viraram `Female`, e `M` virou `Male` (11 registros).
- `Servico_Internet`: `dsl` virou `DSL` (8 registros).

## Resultado

Base final com **2.495 clientes, 16 colunas e nenhum valor nulo**.

## Como executar

```bash
pip install -r ../requirements.txt
cd modulo-14-pre-processamento
jupyter notebook Pre_Processamento_Dados_M14.ipynb
```

**No Google Colab:** abra o notebook, crie a pasta `dados/` no painel de arquivos, envie o `CHURN_TELECON_MOD08_TAREFA.csv` para ela e execute todas as células.

## Tecnologias

Python · pandas · Matplotlib · Seaborn
