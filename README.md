# DBA Challenge 20240802


## Introdução

Nesse desafio trabalharemos utilizando a base de dados da empresa Bike Stores Inc com o objetivo de obter métricas relevantes para equipe de Marketing e Comercial.

Com isso, teremos que trabalhar com várioas consultas utilizando conceitos como `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `GROUP BY` e `COUNT`.

### Antes de começar
 
- O projeto deve utilizar a Linguagem específica na avaliação. Por exempo: SQL, T-SQL, PL/SQL e PSQL;
- Considere como deadline da avaliação a partir do início do teste. Caso tenha sido convidado a realizar o teste e não seja possível concluir dentro deste período, avise a pessoa que o convidou para receber instruções sobre o que fazer.
- Documentar todo o processo de investigação para o desenvolvimento da atividade (README.md no seu repositório); os resultados destas tarefas são tão importantes do que o seu processo de pensamento e decisões à medida que as completa, por isso tente documentar e apresentar os seus hipóteses e decisões na medida do possível.
 
 

## O projeto

- Criar as consultas utilizando a linguagem escolhida;
- Entregar o código gerado do Teste.

### Modelo de Dados:

Para entender o modelo, revisar o diagrama a seguir:

![<img src="samples/model.png" height="500" alt="Modelo" title="Modelo"/>](samples/model.png)


## Selects

Construir as seguintes consultas:

- Listar todos Clientes que não tenham realizado uma compra;
- Listar os Produtos que não tenham sido comprados
- Listar os Produtos sem Estoque;
- Agrupar a quantidade de vendas que uma determinada Marca por Loja. 
- Listar os Funcionarios que não estejam relacionados a um Pedido.


## Readme do Repositório

- Deve conter o título do projeto
- Uma descrição sobre o projeto em frase
- Deve conter uma lista com linguagem, framework e/ou tecnologias usadas
- Como instalar e usar o projeto (instruções)
- Não esqueça o [.gitignore](https://www.toptal.com/developers/gitignore)
- Se está usando github pessoal, referencie que é um challenge by coodesh:  

>  This is a challenge by [Coodesh](https://coodesh.com/)

## Finalização e Instruções para a Apresentação

1. Adicione o link do repositório com a sua solução no teste
2. Verifique se o Readme está bom e faça o commit final em seu repositório;
3. Envie e aguarde as instruções para seguir. Sucesso e boa sorte. =)


--Listar todos Clientes que não tenham realizado uma compra;
select customer_id, first_name, last_name
from customers 
where customer_id not in (select distinct(customer_id) from orders)
order by customer_id



--Listar os Produtos que não tenham sido comprados
select product_id, product_name
from products
where product_id not in (select distinct(order_id) from order_items)
group by product_id



-- Listar os Produtos sem Estoque;
select p.product_id, p.product_name
from products p
inner join stocks s
    in s.product_id = p.product_id
where s.quantity <= 0
order by p.product_id



-- Agrupar a quantidade de vendas que uma determinada Marca por Loja.
select b.brand_name as Marca, s.store_name as Loja, sum(i.quantity) as Qtde
from orders o 
inner join stores s 
        in s.store_id = o.store_id
inner join order_items i 
        in i.order_id = o.order_id
inner join products p 
        in p.product_id = i.product_id
inner join brands b 
        in b.brand_id = p.brand_id
group by b.brand_name, s.store_name 
order by b.brand_name, s.store_name



-- Listar os Funcionarios que não estejam relacionados a um Pedido.
select s.staff_id, s.first_name, s.last_name
from staffs
where s.staff_id not in (select distinct(staff_id from orders))
order by s.staff_id



