# sql-intro
# Manual de SQL com Java e PostgreSQL

## 📌 Conceitos Básicos

- **Campo:** Representa a menor unidade de dados em uma tabela, geralmente uma célula.
- **Tabela:** Estrutura organizada em linhas e colunas para armazenar dados.
- **Linha (Registro):** Conjunto de valores em uma tabela, representando um item.
- **Coluna:** Representa um atributo ou propriedade dos dados armazenados.

---

## 🗃️ Tipos de Dados no Banco de Dados

- **INT:** Números inteiros
- **VARCHAR(n):** Cadeia de caracteres com comprimento máximo de **n**
- **TEXT:** Texto de comprimento variável
- **DATE:** Data (AAAA-MM-DD)
- **BOOLEAN:** Verdadeiro ou Falso
- **NUMERIC:** Números com precisão decimal

---

## 🚀 Conexão em Java com PostgreSQL

Adicione as dependências do PostgreSQL ao seu projeto utilizando o `pom.xml`.

### Exemplo de `pom.xml`
```xml
<dependencies>
  <dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.7.1</version>
  </dependency>
</dependencies>
```

### Código para conexão com o banco de dados PostgreSQL
```java
import java.sql.Connection;
import java.sql.DriverManager;

public class ConexaoPostgreSQL {
    public static Connection conectar() throws Exception {
        String url = "jdbc:postgresql://localhost:5432/seu_banco";
        String usuario = "postgres";
        String senha = "sua_senha";
        return DriverManager.getConnection(url, usuario, senha);
    }
}
```

---

## 🏗️ Comandos SQL Essenciais

### Criar Banco de Dados
```sql
CREATE DATABASE exemplo_db;
```

### Excluir Banco de Dados
```sql
DROP DATABASE exemplo_db;
```

### Criar Tabela
```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100),
    email VARCHAR(150)
);
```

### Excluir Tabela
```sql
DROP TABLE usuarios;
```

### Executar com Java
```java
try (Connection conn = ConexaoPostgreSQL.conectar();
     var stmt = conn.createStatement()) {
    stmt.executeUpdate("CREATE TABLE exemplo (id SERIAL PRIMARY KEY, nome VARCHAR(50))");
    System.out.println("Tabela criada com sucesso!");
}
```

---

## 🗑️ Comando TRUNCATE TABLE
```sql
TRUNCATE TABLE usuarios;
```
### Executar com Java
```java
try (Connection conn = ConexaoPostgreSQL.conectar();
     var stmt = conn.createStatement()) {
    stmt.executeUpdate("TRUNCATE TABLE usuarios");
    System.out.println("Tabela truncada!");
}
```

---

## 📥 Comando INSERT
```sql
INSERT INTO usuarios (nome, email) VALUES ('João Silva', 'joao@email.com');
```
### Executar com Java
```java
try (Connection conn = ConexaoPostgreSQL.conectar();
     var stmt = conn.prepareStatement("INSERT INTO usuarios (nome, email) VALUES (?, ?)");) {
    stmt.setString(1, "Maria Souza");
    stmt.setString(2, "maria@email.com");
    stmt.executeUpdate();
    System.out.println("Usuário inserido com sucesso!");
}
```

---

## 🔎 Comando SELECT
```sql
SELECT * FROM usuarios;
```
### Executar com Java e Exibir no Console
```java
try (Connection conn = ConexaoPostgreSQL.conectar();
     var stmt = conn.createStatement();
     var rs = stmt.executeQuery("SELECT * FROM usuarios")) {
    while (rs.next()) {
        System.out.println("ID: " + rs.getInt("id") + ", Nome: " + rs.getString("nome") + ", Email: " + rs.getString("email"));
    }
}
```

---

## 🖼️ Exibindo Dados em JTable no Java

```java
  String[] colunas={ "Nome", "Salário", "Nascimento", "Data Cadastro"};
  DefaultTableModel model = new DefaultTableModel(colunas, 0);
  model.addRow(new Object[]{"Marcelo","28000,50","1979-11-21","2025-03-24"});
  jTable1.setModel(model);
```

### Exemplo de Exibição com JTable
```java
import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.sql.*;

public class ExibirUsuarios {
    public static void main(String[] args) throws Exception {
        try (Connection conn = ConexaoPostgreSQL.conectar();
             var stmt = conn.createStatement();
             var rs = stmt.executeQuery("SELECT * FROM usuarios")) {
            String[] colunas = {"ID", "Nome", "Email"};
            DefaultTableModel model = new DefaultTableModel(colunas, 0);
            while (rs.next()) {
                model.addRow(new Object[]{rs.getInt("id"), rs.getString("nome"), rs.getString("email")});
            }
            JTable table = new JTable(model);
            JOptionPane.showMessageDialog(null, new JScrollPane(table));
        }
    }
}
```

---

## 🧑‍💻 Conclusão
Este manual apresentou comandos essenciais de SQL e a execução deles com Java e PostgreSQL. Com isso, você pode criar, manipular e exibir dados facilmente.

Caso tenha dúvidas ou precise de mais detalhes, entre em contato com seu instrutor. 😊

