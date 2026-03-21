# 📚 Aula 10 — SELECT Avançado (Parte 2)

---

## 🎯 Objetivos da Aula

* Utilizar o operador **LIKE** para buscas por padrão
* Trabalhar com caracteres coringa (**%** e **_**)
* Aplicar **NOT LIKE** para exclusões
* Utilizar **DISTINCT** para eliminar duplicidades
* Compreender e aplicar **funções de agregação**
* Realizar consultas mais inteligentes e eficientes

---

# 🔎 Buscas por Padrão com LIKE

Até agora usamos comparações exatas com `=`.

Mas em muitos casos queremos buscar **padrões de texto**, como:

```text
- Nomes que começam com "A"
- Emails que terminam com ".com"
- Palavras que contenham determinada letra
```

Para isso usamos o operador:

```sql
LIKE
```
