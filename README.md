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
try {
  Connection conn = ConexaoPostgreSQL.conectar();
  var stmt = conn.createStatement();
  stmt.executeUpdate("CREATE TABLE exemplo (id SERIAL PRIMARY KEY, nome VARCHAR(50))");
  System.out.println("Tabela criada com sucesso!");
}catch(Exception ex){
  System.out.println(ex.getMessage());
}
```

---

## 🗑️ Comando TRUNCATE TABLE
```sql
TRUNCATE TABLE usuarios;
```
### Executar com Java
```java
try {
  Connection conn = ConexaoPostgreSQL.conectar();
  Statement stmt = conn.createStatement();
  stmt.executeUpdate("TRUNCATE TABLE usuarios");
  System.out.println("Tabela truncada!");
}catch(Exception ex){
  System.out.println(ex.getMessage());
}
```

---

## 📥 Comando INSERT
```sql
INSERT INTO usuarios (nome, email) VALUES ('João Silva', 'joao@email.com');
```
### Executar com Java
```java
try {
  Connection conn = ConexaoPostgreSQL.conectar();
  Statement stmt = conn.prepareStatement("INSERT INTO usuarios (nome, email) VALUES (?, ?)");
  stmt.setString(1, "Maria Souza");
  stmt.setString(2, "maria@email.com");
  stmt.executeUpdate();
  System.out.println("Usuário inserido com sucesso!");
}catch(Exception ex){
  System.out.println(ex.getMessage());
}
```

---

## 🔎 Comando SELECT
```sql
SELECT * FROM usuarios;
```
### Executar com Java e Exibir no Console
```java
try {
  Connection conn = ConexaoPostgreSQL.conectar();
  Statement stmt = conn.createStatement();
  ResultSet rs = stmt.executeQuery("SELECT * FROM usuarios");
  while (rs.next()) {
    System.out.println("ID: " + rs.getInt("id") + ", Nome: " + rs.getString("nome") + ", Email: " + rs.getString("email"));
  }
}catch(Exception ex){
  System.out.println(ex.getMessage());
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

ou 
```java
Object[] coluna=new Object[4];
coluna[0] = "Nome";
coluna[1] = "Salário";
coluna[2] = "Nascimento";
coluna[3] = "Data Cadastro";

Object[][] usuario=new Object[10][4];
usuario[0][0]="Marcelo";
usuario[0][1]="28000,50";
usuario[0][2]="1979-11-21";
usuario[0][3]="2025-03-24";

jTable1.setModel(new DefaultTableModel(usuario,coluna));

```

### Exemplo de Exibição com JTable
```java
import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.sql.*;

//...
try {
    Connection conn = ConexaoPostgreSQL.conectar();
    var stmt = conn.createStatement();
    var rs = stmt.executeQuery("SELECT * FROM usuarios");
    String[] colunas = {"ID", "Nome", "Email"};
    DefaultTableModel model = new DefaultTableModel(colunas, 0);
    while (rs.next()) {
        model.addRow(new Object[]{rs.getInt("id"), rs.getString("nome"), rs.getString("email")});
    }
    JTable table = new JTable(model);
    JOptionPane.showMessageDialog(null, new JScrollPane(table));
}catch(Exception ex){
  System.out.println(ex.getMessage());
}
```

---

### Exemplo de DELETE
```java
//...
try {
    int id = 1; 
    String comando = "DELETE FROM veiculos WHERE id = ?";
    PreparedStatement pstmt;
    pstmt = conexao.prepareStatement(comando);
    pstmt.setInt(1, id);
    pstmt.executeUpdate();
    JOptionPane.showMessageDialog(this, "Apagado com sucesso");
} catch (Exception e) {
    JOptionPane.showMessageDialog(this, "Erro ao apagar");
}
```
exemplo 2

```java
try {
    String id_ = jLabel1.getText();
    int id = Integer.parseInt(id_);
    String comando = "DELETE FROM veiculos WHERE id = ?";
    PreparedStatement pstmt;
    pstmt = conexao.prepareStatement(comando);
    pstmt.setInt(1, id);
    pstmt.executeUpdate();
    JOptionPane.showMessageDialog(this, "Apagado com sucesso");
    jButton5.doClick();
} catch (Exception e) {
    JOptionPane.showMessageDialog(this, "Erro ao apagar");
}
```
## Passo a Passo para criar um projeto no Netbeans com acesso a PostgreSQL
# 1 Criar um projeto Maven
# 2 Editar o arquivo pom.xml
Criar a tag 
```
<dependencies>
```
e adicionar a dependência do banco de dados PostgreSQL.
```xml
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.postgresql/postgresql -->
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.7.5</version>
        </dependency>
    </dependencies>
```
# 3 Criar um JFrame, apagar a classe com o main e configurar o JFrame como classe principal:
Clicar com botão direito no projeto, clicar em Propriedades, clicar em Run, ir em main class, clicar em Browse... selecionar o JFrame criado.

# 4 Iniciar o banco com o Start PostgreSQL
Para se conectar no banco vamos colocar o código no JFrame principal:
Nosso JFrame vai ficar com este método "conecta()"
OBS. a primeira linha não deve ser copiada, serve apenas para você se localizar.

```java
public class Tela extends javax.swing.JFrame {
    public static Connection conexao;
    
    public void conecta(){
        System.out.println("Conetando o banco");
        try {
            Class.forName("org.postgresql.Driver");
            String url = "jdbc:postgresql://localhost:5432/postgres";
            String user = "postgres";
            String pass = "feevale";
            conexao = DriverManager.getConnection(url, user, pass);
            botaoConecta.setBackground(Color.green);
            labelConecta.setText("Conectado");
        } catch (Exception e) {
            botaoConecta.setBackground(Color.red);
            labelConecta.setText("Desconectado");
        }
    }
    ...
```
O método "conecta()" é executado tanto quando o progama é aberto quanto quando clicamos no botão de status da conexão.

# 5 Criar os JInternalFrame e fazer as opções de menu para chamar eles, com o seguinte comando:
```java
    TelaInsert i;
    i = new TelaInsert();
    jDesktopPane2.add(i);
    i.setVisible(true);
```
O comando acima supoe que temos um JInternalFrame com o nome TelaInsert.
Precisamos também tem em nosso JFrame um JDesktopPane, no caso nosso JDesktopPane, é o jDesktopPane2.

# 6 na tela do insert supondo que temos campos para serem preenchidos.

Podemos colocar o seguinte código no botão insert
```java
try {
    String comando;
    comando = "INSERT into veiculos "
            + "(modelo) VALUES "
            + "(?)";
    PreparedStatement pstmt;
    pstmt = Tela.conexao.prepareStatement(comando);
    pstmt.setString(1, txtModelo.getText());
    pstmt.executeUpdate();
    JOptionPane.showMessageDialog(this, "Inserido com Sucesso");
} catch (Exception e) {
    JOptionPane.showMessageDialog(this, "Erro na inserção "+e.getMessage());
}
```
OBS. para funcionar, precisamos ter a conexão feita no Tela.
OBS2. Neste insert temos uma informação que foi digitada pelo usuário, no caso foi o modelo do veículo.
Tal informação foi digitada em um Campo de Texto com o nome "txtModelo".

# 7 na tela de Seleção podemos colocar o seguinte código





