# Recency-Frequency-Value-RFV
Essa segmentação agrupa os clientes de acordo com o comportamento de compra, permitindo ações de marketing e CRM direcionados.

## A aplicação inicia com uma breve introdução sobre o RFV e sua importância na segmentação de clientes.
```python
# Título principal da aplicação
    st.write("""# RFV

    RFV significa recência, frequência, valor e é utilizado para segmentação de clientes baseado no comportamento 
    de compras dos clientes e agrupa eles em clusters parecidos. Utilizando esse tipo de agrupamento podemos realizar 
    ações de marketing e CRM melhores direcionadas, ajudando assim na personalização do conteúdo e até a retenção de clientes.

    Para cada cliente é preciso calcular cada uma das componentes abaixo:

    - Recência (R): Quantidade de dias desde a última compra.
    - Frequência (F): Quantidade total de compras no período.
    - Valor (V): Total de dinheiro gasto nas compras do período.

    E é isso que iremos fazer abaixo.
    """)
    st.markdown("---")
  ```
![RFV1](https://github.com/TKlebis/Recency-Frequency-Value-RFV/assets/130613291/51c87f9e-ce66-4cdf-8608-3e8b0de91673)

## O usuário pode carregar um arquivo de dados no formato CSV ou Excel através de um botão na barra lateral da aplicação.

```python
# Botão para carregar arquivo na aplicação
    st.sidebar.write("## Suba o arquivo")
    data_file_1 = st.sidebar.file_uploader("Bank marketing data", type = ['csv','xlsx'])
```
![RFV2](https://github.com/TKlebis/Recency-Frequency-Value-RFV/assets/130613291/31d0e9e7-d3d6-489a-a67e-c34a8df0e720)

## Após o carregamento do arquivo, a função main()realiza as seguintes etapas de processamento:

**Recência (R):** É calculado o número de dias desde a última compra para cada cliente, utilizando a coluna "DiaCompra" do dataframe de compras. O resultado é armazenado no dataframe df_recencia.
```python
st.write('## Recência (R)')

        
        dia_atual = df_compras['DiaCompra'].max()
        st.write('Dia máximo na base de dados: ', dia_atual)

        st.write('Quantos dias faz que o cliente fez a sua última compra?')

        df_recencia = df_compras.groupby(by='ID_cliente', as_index=False)['DiaCompra'].max()
        df_recencia.columns = ['ID_cliente','DiaUltimaCompra']
        df_recencia['Recencia'] = df_recencia['DiaUltimaCompra'].apply(lambda x: (dia_atual - x).days)
        st.write(df_recencia.head())

        df_recencia.drop('DiaUltimaCompra', axis=1, inplace=True)
  ```
  ![RFV3](https://github.com/TKlebis/Recency-Frequency-Value-RFV/assets/130613291/9f011ff3-8908-4d7b-a5d0-ab16530ebc2a)

**Frequência (F):** É contabilizado o número total de compras de cada cliente no período, utilizando a coluna "CodigoCompra" do dataframe de compras. O resultado é armazenado no dataframe df_frequencia.
```pyhton
st.write('## Frequência (F)')
        st.write('Quantas vezes cada cliente comprou com a gente?')
        df_frequencia = df_compras[['ID_cliente','CodigoCompra']].groupby('ID_cliente').count().reset_index()
        df_frequencia.columns = ['ID_cliente','Frequencia']
        st.write(df_frequencia.head())
 ```
![RFV4](https://github.com/TKlebis/Recency-Frequency-Value-RFV/assets/130613291/f06dd420-ac02-4973-ac82-dc9bdae96f31)
 
**Valor (V):** É calculado o valor total gasto por cada cliente no período, utilizando a coluna "ValorTotal" do dataframe de compras. O resultado é armazenado no dataframe df_valor.
```python
st.write('## Valor (V)')
        st.write('Quanto que cada cliente gastou no periodo?')
        df_valor = df_compras[['ID_cliente','ValorTotal']].groupby('ID_cliente').sum().reset_index()
        df_valor.columns = ['ID_cliente','Valor']
        st.write(df_valor.head())
 ```
 ![RFV5](https://github.com/TKlebis/Recency-Frequency-Value-RFV/assets/130613291/2f562d76-536e-43a8-8d9c-a6cc58cd0312)

Os dataframes df_recencia, df_frequenciae df_valorsão combinados em um único dataframe df_RFV, que contém as informações de recência, frequência e valor para cada cliente.

Os quartis são calculados para cada componente (recência, frequência e valor) do RFV, utilizando o método quantile()dos pandas. Os quartis são armazenados no dataframe quartis.

Com base nos quartis, cada cliente é classificado em um dos quatro grupos (A, B, C, D) para cada componente do RFV, utilizando as funções recencia_class()e freq_val_class(). O resultado é adicionado como colunas extras no dataframe df_RFV.

O RFV Score é calculado somando as classificações de recência, frequência e valor para cada cliente, criando um indicador geral de comportamento. O RFV Score é adicionado como coluna extra no dataframe df_RFV.


https://github.com/TKlebis/Recency-Frequency-Value-RFV/assets/130613291/2c6f32c6-9640-48a3-9f68-fd388f038ca2



   
