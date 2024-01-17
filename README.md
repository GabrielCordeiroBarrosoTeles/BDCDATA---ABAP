# BDCDATA - ABAP

A estrutura BDCDATA é utilizada no SAP (Sistemas, Aplicações e Produtos) para carregar dados usando a Comunicação de Dados em Lotes (BDC) no contexto do Gerenciamento de Telas SAP (SHBD). Essencialmente, é um tipo de tabela interna no SAP e desempenha um papel crucial na captura e armazenamento de dados que precisam ser inseridos no sistema SAP.

Vamos percorrer a estrutura e seus campos com exemplos e comentários em uma representação de pseudo-código:

```ABAP
TYPES: BEGIN OF BDCDATA,
         PROGRAM TYPE PROGRAM,
         DYNPRO TYPE DYNPRO,
         DYNBEGIN TYPE CHAR1,
         FNAM TYPE FIELDNAME,
         FVAL TYPE STRING,
       END OF BDCDATA.

DATA: lt_bdcdata TYPE TABLE OF BDCDATA,
      ls_bdcdata TYPE BDCDATA.

* Exemplo 1: Preenchendo BDCDATA para uma tela com um valor de campo
CLEAR ls_bdcdata.
ls_bdcdata-PROGRAM = 'SAPLSMTR_NAVIGATION'.
ls_bdcdata-DYNPRO = '0100'.
ls_bdcdata-DYNBEGIN = 'X'.
ls_bdcdata-FNAM = 'BDC_FIELD'.
ls_bdcdata-FVAL = 'Valor de Exemplo'.
APPEND ls_bdcdata TO lt_bdcdata.

* Exemplo 2: Preenchendo BDCDATA para uma tela sem um valor de campo
CLEAR ls_bdcdata.
ls_bdcdata-PROGRAM = 'SAPLSMTR_NAVIGATION'.
ls_bdcdata-DYNPRO = '0100'.
ls_bdcdata-DYNBEGIN = ' '.
ls_bdcdata-FNAM = 'OUTRO_CAMPO'.
ls_bdcdata-FVAL = ''.
APPEND ls_bdcdata TO lt_bdcdata.

* Exemplo 3: Preenchendo BDCDATA para uma tela com código de função
CLEAR ls_bdcdata.
ls_bdcdata-PROGRAM = 'SAPLSMTR_NAVIGATION'.
ls_bdcdata-DYNPRO = '0100'.
ls_bdcdata-DYNBEGIN = ' '.
ls_bdcdata-FNAM = 'BDC_OKCODE'.
ls_bdcdata-FVAL = '/00'.
APPEND ls_bdcdata TO lt_bdcdata.

* Em um programa SAP real, você populava lt_bdcdata com base em seus dados e lógica.

* Após popular lt_bdcdata, você pode usá-lo na criação da sessão BDC ou CALL TRANSACTION.

* Exemplo de uso de lt_bdcdata para criação de sessão BDC
CALL FUNCTION 'BDC_OPEN_GROUP'.
CALL FUNCTION 'BDC_INSERT' EXPORTING
                  TC_GROUP   = 'NOME_DA_SESSAO'
                  TC_OBJECT  = 'SEU_CODIGO_DE_TRANSACAO'
                  TC_CUA     = ' '.
LOOP AT lt_bdcdata INTO ls_bdcdata.
  CALL FUNCTION 'BDC_INSERT' EXPORTING
                    TC_DATA    = ls_bdcdata.
ENDLOOP.
CALL FUNCTION 'BDC_CLOSE_GROUP'.

* Exemplo de uso de lt_bdcdata com CALL TRANSACTION
CALL TRANSACTION 'SEU_CODIGO_DE_TRANSACAO' USING lt_bdcdata
                      MODE 'A' " ou 'N' para modo de exibição
                      UPDATE 'S'.
```

Lembre-se de que, em um programa SAP real, você precisaria substituir marcadores de posição, como 'NOME_DA_SESSAO' e 'SEU_CODIGO_DE_TRANSACAO', com suas informações reais de sessão e transação. Além disso, os códigos de função como '/00' no campo FNAM são usados para executar ações específicas na transação, como salvar dados.
