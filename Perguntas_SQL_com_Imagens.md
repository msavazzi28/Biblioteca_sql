
# 📘 Perguntas SQL com Respostas Visuais

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
