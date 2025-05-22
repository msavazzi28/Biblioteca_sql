
# 📘 Perguntas SQL

---

### 1 - Quantos livros existem cadastrados?

```sql
SELECT COUNT(*) FROM livros;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%201.png" alt="Resposta 1" width="600"/>
</p>

---

### 2 - Listar todos os usuários ordenados pelo nome.

```sql
SELECT DISTINCT nome FROM usuarios;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%202.png" alt="Resposta 2" width="600"/>
</p>

---

### 3 - Mostrar todos os empréstimos realizados no mês atual.

```sql
SELECT id_livro, data_emprestimo 
FROM emprestimos
WHERE EXTRACT(MONTH FROM data_emprestimo) = 5 
  AND EXTRACT(YEAR FROM data_emprestimo) = 2025;

-- OU

SELECT id_livro, data_emprestimo
FROM emprestimos
WHERE data_emprestimo >= '2025-05-01'
  AND data_emprestimo < '2025-06-01';
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%203.png" alt="Resposta 3" width="600"/>
</p>

---

### 4 - Mostrar os títulos dos livros emprestados.

```sql
SELECT DISTINCT l.titulo
FROM emprestimos AS e
JOIN livros AS l ON e.id_livro = l.id_livro;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%204.png" alt="Resposta 4" width="600"/>
</p>

---

### 5 - Listar o nome dos usuários que pegaram livros emprestados.

```sql
SELECT DISTINCT u.nome
FROM emprestimos AS e
JOIN usuarios AS u ON e.id_usuario = u.id_usuario;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%205.png" alt="Resposta 5" width="600"/>
</p>

---

<!-- Continua até a 20ª pergunta, por questão de espaço vamos dividir -->

---

### 6 - Quantos livros diferentes foram emprestados ao menos uma vez?

```sql
SELECT COUNT(DISTINCT id_livro) FROM emprestimos;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%206.png" alt="Resposta 6" width="600"/>
</p>

---

### 7 - Quantos usuários distintos fizeram empréstimos?

```sql
SELECT COUNT(DISTINCT id_usuario) FROM emprestimos;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%207.png" alt="Resposta 7" width="600"/>
</p>

---

### 8 - Listar os 10 autores com mais livros cadastrados.

```sql
SELECT autor, COUNT(*) AS total_livros
FROM livros
GROUP BY autor
ORDER BY total_livros DESC
LIMIT 10;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%208.png" alt="Resposta 8" width="600"/>
</p>

---

### 9 - Mostrar os 5 livros mais emprestados.

```sql
SELECT COUNT(e.id_livro) AS contagem, l.titulo
FROM emprestimos AS e
JOIN livros AS l ON e.id_livro = l.id_livro
GROUP BY l.titulo
ORDER BY contagem DESC
FETCH FIRST 5 ROWS ONLY;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%209.png" alt="Resposta 9" width="600"/>
</p>

---

### 10 - Listar os usuários que mais fizeram empréstimos.

```sql
SELECT COUNT(e.id_usuario) AS contagem, u.nome
FROM emprestimos AS e
JOIN usuarios AS u ON e.id_usuario = u.id_usuario
GROUP BY u.nome
ORDER BY contagem DESC;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2010.png" alt="Resposta 10" width="600"/>
</p>

---

### 11 - Listar todos os empréstimos ativos hoje (data_devolucao > CURRENT_DATE).

```sql
SELECT l.titulo, e.data_emprestimo
FROM livros AS l
JOIN emprestimos AS e ON e.id_livro = l.id_livro
WHERE e.data_devolucao > CURRENT_DATE;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2011.png" alt="Resposta 11" width="600"/>
</p>

---

### 12 - Listar os livros que nunca foram emprestados.

```sql
SELECT l.id_livro, l.titulo
FROM livros AS l
LEFT JOIN emprestimos AS e ON l.id_livro = e.id_livro
WHERE e.id_livro IS NULL;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2012.png" alt="Resposta 12" width="600"/>
</p>

---

### 13 - Mostrar os usuários que emprestaram mais de 5 livros.

```sql
SELECT u.nome, COUNT(*) AS contagem
FROM emprestimos AS e
JOIN usuarios AS u ON u.id_usuario = e.id_usuario
GROUP BY u.nome
HAVING COUNT(*) > 5
ORDER BY contagem DESC;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2013.png" alt="Resposta 13" width="600"/>
</p>

---

### 14 - Calcular a média de tempo de empréstimo (em dias) por usuário.

```sql
SELECT u.nome, ROUND(AVG(e.data_devolucao - e.data_emprestimo), 2) AS dias_emprestimos
FROM emprestimos AS e
JOIN usuarios AS u ON u.id_usuario = e.id_usuario
GROUP BY u.nome
ORDER BY dias_emprestimos DESC;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2014.png" alt="Resposta 14" width="600"/>
</p>

---

### 15 - Listar os livros mais emprestados por gênero.

```sql
WITH livros_por_genero AS (
  SELECT
    l.genero,
    l.titulo,
    COUNT(*) AS total_emprestimos,
    RANK() OVER (PARTITION BY l.genero ORDER BY COUNT(*) DESC) AS posicao
  FROM emprestimos AS e
  JOIN livros AS l ON l.id_livro = e.id_livro
  GROUP BY l.genero, l.titulo
)
SELECT genero, titulo, total_emprestimos
FROM livros_por_genero
WHERE posicao = 1;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2015.png" alt="Resposta 15" width="600"/>
</p>

---

### 16 - Listar os 5 usuários que mais pegaram livros de “Ficção”.

```sql
WITH contagem_usuario AS (
  SELECT e.id_usuario, COUNT(*) AS contagem
  FROM livros AS l
  JOIN emprestimos AS e ON e.id_livro = l.id_livro
  WHERE l.genero = 'Ficção'
  GROUP BY e.id_usuario
)
SELECT u.nome, cu.contagem
FROM contagem_usuario AS cu
JOIN usuarios AS u ON u.id_usuario = cu.id_usuario
ORDER BY cu.contagem DESC
LIMIT 5;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2016.png" alt="Resposta 16" width="600"/>
</p>

---

### 17 - Encontrar o autor cujo livro teve o maior tempo total emprestado (em dias somados).

```sql
SELECT l.autor, ROUND(SUM(e.data_devolucao - e.data_emprestimo), 2) AS dias_emprestimos
FROM emprestimos AS e
JOIN livros AS l ON l.id_livro = e.id_livro
GROUP BY l.autor
ORDER BY dias_emprestimos DESC
LIMIT 1;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2017.png" alt="Resposta 17" width="600"/>
</p>

---

### 18 - Para cada mês do último ano, mostrar quantos empréstimos foram feitos.

```sql
SELECT TO_CHAR(data_emprestimo, 'YYYY-MM') AS mes,
       COUNT(*) AS total_emprestimos
FROM emprestimos
WHERE data_emprestimo >= CURRENT_DATE - INTERVAL '12 months'
GROUP BY mes
ORDER BY mes;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2018.png" alt="Resposta 18" width="600"/>
</p>

---

### 19 - Listar o livro com maior quantidade de empréstimos por mês.

```sql
WITH contagem_por_mes AS (
  SELECT
    l.titulo,
    EXTRACT(YEAR FROM e.data_emprestimo) AS ano,
    EXTRACT(MONTH FROM e.data_emprestimo) AS mes,
    COUNT(*) AS total_emprestimos,
    RANK() OVER (
      PARTITION BY EXTRACT(YEAR FROM e.data_emprestimo), EXTRACT(MONTH FROM e.data_emprestimo)
      ORDER BY COUNT(*) DESC
    ) AS posicao
  FROM emprestimos AS e
  JOIN livros AS l ON l.id_livro = e.id_livro
  GROUP BY l.titulo, ano, mes
)
SELECT titulo, ano, mes, total_emprestimos
FROM contagem_por_mes
WHERE posicao = 1
ORDER BY ano, mes;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2019.png" alt="Resposta 19" width="600"/>
</p>

---

### 20 - Listar os usuários que emprestaram livros consecutivamente (sem intervalo entre devolução e novo empréstimo).

```sql
SELECT 
    u.nome,
    e.id_usuario,
    e.id_emprestimo,
    e.data_emprestimo,
    e.data_devolucao,
    e.devolucao_anterior
FROM (
    SELECT *,
           LAG(data_devolucao) OVER (PARTITION BY id_usuario ORDER BY data_emprestimo) AS devolucao_anterior
    FROM emprestimos
) AS e
JOIN usuarios AS u ON u.id_usuario = e.id_usuario
WHERE e.data_emprestimo = e.devolucao_anterior
ORDER BY e.id_usuario, e.data_emprestimo;
```

<p align="center">
  <img src="Fotos%20Respostas/Resposta%2020.png" alt="Resposta 20" width="600"/>
</p>
