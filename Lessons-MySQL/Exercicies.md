Exercício 1

"Uma lista com o nome de todas as gafanhotas"

select * from gafanhotos where sexo = 'F'; ou
select nome from gafanhotos where sexo = 'F';

---
Exercício 2
"Uma lista com os dados de todos aqueles que nasceram entre 1/jan/2000 e 31/dez/2015"

select * from gafanhotos where nascimento between '2000-01-01' and '2015-12-31';

ou

select * from gafanhotos
where nascimento between '2000-01-01' and '2015-12-31'
order by nascimento;
---

Exercício 3

"Uma lista com o nome de todos os homens que trabalham como Programadores"

select nome, profissao from gafanhotos
where sexo = 'M' and profissao = 'programador';

---

Exercício 4

'Uma lista com os dados de todas as mulheres que nasceram no Brasil e que tem seu nome iniciando com a letra J"

select nome from gafanhotos
where nacionalidade = 'Brasil' and nome like 'J%' and sexo = 'f';

---
Exercício 5
"Uma lista com o nome e nacionalidade de todos os homens que tem silva no nome, não nasceram no Brasil e pesam menos de 100kg"

select nome, nacionalidade from gafanhotos
where (nacionalidade != 'Brasil') and (sexo = 'M') and (peso < 100) and (nome like '%silva%');

---

Exercício 6

"Qual é a maior altura entre gafanhotos homens que moram no Brasil"

select max(altura) from gafanhotos where (sexo = 'M') and (nacionalidade != 'Brasil');

---
Exercício 7

"Qual a media de peso dos gafanhotos cadastrados?"

select avg(peso) from gafanhotos;

---

Exercício 8

"Qual é o menor peso entre as gafanhotas mulheres que nasceram fora do Brasil e entre 01/jan/1990 e 31/dez/2000"

select min(peso) from gafanhotos where (nacionalidade != 'Brasil') and nascimento between '1990-01-01' and '2000-12-31';

---

Exercício 9
"Quantas gafanhotos mulheres tem mais de 1.90m de altura?"

select count(altura) from gafanhotos where (sexo = 'F') and (altura > 1.90);

---

Exercício 10
"Uma lista com as profissões dos gafanhotos e seus respectivos quantitativos"

select profissao, count(*) from gafanhotos
group by profissão

---

Exercício 11
"Quantos gafanhotos homens e quantas mulheres nasceram após 1/jan/2005"

select sexo, count(nascimento) from gafanhotos
where nascimento > '2005-01-01'
group by sexo;

---

Exercício 12
"Uma lista com os gafanhotos que nasceram fora do Brasil, mostrando o páis de origem e o total de pessoas nascidas lá. Só nos interessam os países que tiveram mais de 3 gafanhotos com essa nacionalidade"

select nacionalidade, count(*) from gafanhotos
where not nacionalidade = 'Brasil'
group by nacionalidade
having count(nacionalidade) > 3;

---

Exercício 13

" Uma lista agrupada pela altura dos gafanhotos, mostrando quantas pessoas pesam mais de 100kg e que estão acima da média de altura de todos os cadastrados".

select altura, count(*) from gafanhotos
where peso > '100'
group by altura
having altura > (select avg(altura) from gafanhotos);
