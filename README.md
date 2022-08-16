# Sprint 1 - SQL

> Foi utilizado de um banco de dados disponibilizado para o programa de bolsa da Compass para a execução.

## Listar todos os livros publicados após 2014


```sh
SELECT 
    Titulo, Publicacao
FROM
    LIVRO
WHERE
    YEAR(Publicacao) > 2014
    ORDER BY Publicacao;
```

## Listar os 10 livros mais caros
```sh
SELECT 
    Titulo, Valor
FROM
    LIVRO
ORDER BY Valor DESC
LIMIT 10;
```
## Listar as 5 editoras que mais tem livros na biblioteca
```sh
SELECT 
    Nome AS Editora, COUNT(*) AS Cont_Livros
FROM
    LIVRO AS A
        INNER JOIN
    EDITORA AS B ON A.Editora = B.CodEditora
GROUP BY CodEditora
ORDER BY Cont_Livros DESC
LIMIT 5;
```
## Listar a quantidade de publicações de cada autor
```sh
SELECT 
    Nome, COUNT(Publicacao) AS Cont
FROM
    AUTOR AS A
        LEFT JOIN
    LIVRO AS B ON A.CodAutor = B.Autor
GROUP BY Nome
ORDER BY Nome;
```
## Listar a quantidade de publicações de cada editora
```sh
SELECT 
    CodEditora, Nome, COUNT(Publicacao) AS Cont
FROM
    EDITORA AS A
        INNER JOIN
    LIVRO AS B ON A.CodEditora = B.Editora
GROUP BY CodEditora;
```
## Listar qual é o autor com mais publicações
```sh
SELECT 
    Nome, COUNT(*) AS Cont
FROM
    LIVRO AS A
        INNER JOIN
    AUTOR AS B ON A.Autor = B.CodAutor
GROUP BY A.Autor
ORDER BY Cont DESC
LIMIT 1;
```
## Listar qual é o autor com menos ou nenhuma publicação
```sh
SELECT 
    Nome, COUNT(Publicacao) AS Cont
FROM
    AUTOR AS A
        LEFT JOIN
    LIVRO AS B ON A.CodAutor = B.Autor
GROUP BY Nome
HAVING COUNT(Publicacao) < 1
ORDER BY Cont;
```
## Selecione o nome e o código do vendedor com o maior número de vendas.
```sh
SELECT 
    B.NmVdd AS Nome, A.CdVdd AS Cod_vend, COUNT(*) AS Tot_Vend
FROM
    TbVendas AS A
        INNER JOIN
    TbVendedor AS B ON A.CdVdd = B.CdVdd
WHERE
    status IN ('Concluido')
GROUP BY A.CdVdd
ORDER BY Tot_Vend DESC
LIMIT 1;
```
## Selecione o produto mais vendido entre as datas de 2014-02-03 até 2018-02-02.
```sh
SELECT 
    CdPro, NmPro, SUM(Qtd) AS Tot_vend
FROM
    TbVendas
WHERE
    DtVen BETWEEN '2014-02-03' AND '2018-02-02'
        AND status IN ('Concluido')
GROUP BY NmPro
ORDER BY Tot_vend DESC;
```
## Calcule a comissão dos vendedores.
```sh
SELECT 
    A.CdVdd,
    B.NmVdd AS Nome,
    SUM(A.Qtd) AS Tot_vend,
    FORMAT(SUM(A.Qtd * A.VrUnt), 2) AS Tot_Val,
    B.PercComissao,
    FORMAT(SUM(A.Qtd * A.VrUnt) * (B.PercComissao / 100),
        2) AS Comissao
FROM
    TbVendas AS A
        INNER JOIN
    TbVendedor AS B ON A.CdVdd = B.CdVdd
WHERE
    status IN ('Concluido')
GROUP BY A.CdVdd
ORDER BY SUM(A.Qtd * A.VrUnt) DESC;
```
## Selecione o cliente que mais gastou.
```sh
SELECT 
    CdCli AS Cod_Cliente,
    NmCli AS Nome_Cliente,
    FORMAT((SUM(Qtd) * VrUnt), 2) AS Gasto
FROM
    TbVendas
WHERE
    status IN ('Concluido')
GROUP BY CdCli
ORDER BY (SUM(Qtd) * VrUnt) DESC
LIMIT 1;
```
## Selecione a escola que mais gastou.
```sh
SELECT 
    B.InepEscola AS Escola,
    FORMAT(SUM(A.Qtd * A.VrUnt), 2) AS Tot_Val
FROM
    TbVendas AS A
        INNER JOIN
    TbDependente AS B ON A.CdVdd = B.CdVdd
WHERE
    status LIKE '%Concluido%'
        AND Deletado IS FALSE
GROUP BY B.InepEscola
ORDER BY SUM(A.Qtd * A.VrUnt) DESC;
```
## Selecione os 10 produtos menos vendidos por ecommerce e pela matriz.
```sh
SELECT 
    NmPro AS Nome_Prod,
    COUNT(*) AS Tot_Vend,
    NmCanalVendas AS Canal
FROM
    TbVendas
WHERE
    status IN ('Concluido')
        AND NmCanalVendas = 'Matriz'
        OR NmCanalVendas = 'Ecommerce'
GROUP BY NmPro , Canal
ORDER BY Tot_Vend DESC;
```
## Calcule a média de gasto por estado.
```sh
SELECT 
    Estado,
    SUM(Qtd * VrUnt) AS Tot_gasto,
    ROUND((SUM(Qtd * VrUnt) / COUNT(Estado)), 2) AS Med_gasto
FROM
    TbVendas
WHERE
    status IN ('Concluido')
GROUP BY Estado
ORDER BY Med_gasto DESC;
```
## Selecione todos os registros deletados.
```sh
SELECT 
    NmCli AS Nome_Cliente,
    Estado,
    NmPro AS Nome_Prod,
    Qtd,
    VrUnt,
    CdVdd,
    NmCanalVendas AS Canal_Vendas,
    Deletado
FROM
    TbVendas
WHERE
    deletado IS TRUE
ORDER BY CdVen;
```
## Calcule a média da quantidade vendida de cada produto por estado.
```sh
SELECT 
    Nmpro AS NomeProduto,
    Estado,
    FORMAT(SUM(Qtd) / COUNT(NmPro), 2) AS Media
FROM
    TbVendas
WHERE
    status IN ('Concluido')
GROUP BY Cidade, Estado, NmPro
ORDER BY Nmpro;
```
## Selecione os gastos por dependente.
```sh
SELECT 
    B.NmDep AS Dependentes,
    FORMAT((SUM(A.Qtd) * A.VrUnt), 2) AS Tot_Val
FROM
    TbVendas AS A
        INNER JOIN
    TbDependente AS B ON A.CdVdd = B.CdVdd
WHERE
    status LIKE '%Concluido%'
        AND Deletado IS FALSE
GROUP BY B.NmDep
ORDER BY B.NmDep;
```
