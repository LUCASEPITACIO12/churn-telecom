# Módulo 16 – Aprendizado supervisionado x não supervisionado

[← Voltar ao projeto](../README.md)

Atividade do curso de Ciência de Dados da [EBAC](https://ebaconline.com.br/). O exercício pede para diferenciar os dois tipos de aprendizado de máquina, trazer exemplos de uso em problemas de negócio e indicar qual deles serve para prever o churn da base de clientes de telecom trabalhada desde o módulo 14.

## Perguntas respondidas

1. Diferença entre aprendizado supervisionado e não supervisionado
2. Exemplos de problemas de negócio com aprendizado supervisionado (crédito, fraude em cartão e previsão de demanda)
3. Exemplo de problema de negócio com aprendizado não supervisionado (segmentação de clientes e detecção de anomalias)
4. Por que a base de teste não deve conter os rótulos a serem previstos
5. Tipo de aprendizado ideal para prever o churn da base do módulo 14

As respostas completas estão em [`respostas.md`](respostas.md), que abre direto aqui no GitHub. A versão entregue na plataforma está em [`documentos/`](documentos/), em PDF e DOCX.

## Conclusão sobre o churn

A base tem 2.495 clientes e uma coluna `Churn` que registra quem cancelou. Como existe esse histórico com a resposta e o objetivo é prever o mesmo desfecho para clientes ativos, o problema é de aprendizado supervisionado, do tipo classificação binária. A análise exploratória do módulo 15 mostrou variáveis com forte relação com o cancelamento, como tipo de contrato, tempo de casa e suporte técnico. Os cuidados para a modelagem são o desbalanceamento da classe (cerca de 26% de churn), que pede divisão estratificada e atenção ao recall e ao F1, e a correlação de 0,83 entre `Tempo_como_Cliente` e `Total_Pago`.

## Estrutura

```
.
├── README.md
├── respostas.md
└── documentos/
    ├── Atividade_Aprendizado_Supervisionado_x_Nao_Supervisionado.pdf
    └── Atividade_Aprendizado_Supervisionado_x_Nao_Supervisionado.docx
```

## Autor

Lucas Farias Epitácio de Almeida · [GitHub](https://github.com/LUCASEPITACIO12)
