# LivrariaVirtual
	Hoje dia 24/10/2021 finalmente conseguimos usar o GIT e GitHub 
	da forma que o professor orientou na aula. Não merecemos Palmas,
	merecemos o Tocantins inteiro. Clap Clap Clap Clap Clap Clap...



# Classes do Projeto LivrariaVirtual e seus códigos:

				Feito em Linguagem Java
# Classe Livro
package com.mycompany.lojavirtual;

public class Livro {

    private String nomep;
    private double precoUnitario;

    public Livro(String nomep, double precoUnitario) {
        this.nomep = nomep;
        this.precoUnitario = precoUnitario;
    }

    public String getNomeP() {
        return nomep;
    }

    public void setNomeP(String nomep) {
        this.nomep = nomep;
    }

    public double getPrecoUnitario() {
        return precoUnitario;
    }

    public void setPrecoUnitario(double precoUnitario) {
        this.precoUnitario = precoUnitario;
    }

    public String toString() {
        return "{Produto: " + this.getNomeP() + ", Preço Unitário: " + this.getPrecoUnitario() + "}";

    }
}

# Classe Cliente
package com.mycompany.lojavirtual;

public class Cliente {

    private String nomeC;
    private int idade;
    private String telefone;
    private String email;

    public Cliente(String nomeC, int idade, String telefone, String email) {
        this.nomeC = nomeC;
        this.idade = idade;
        this.telefone = telefone;
        this.email = email;
    }

    public String getNomeC() {
        return nomeC;
    }

    public void setNomeC(String nomeC) {
        this.nomeC = nomeC;
    }

    public int getIdade() {
        return idade;
    }

    public void setIdade(int idade) {
        this.idade = idade;
    }

    public String getTelefone() {
        return telefone;
    }

    public void setTelefone(String telefone) {
        this.telefone = telefone;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "{Cliente: " + this.getNomeC() + ", idade: " + this.getIdade() + ", telefone: " + this.getTelefone() + ", e-mail:" + this.getEmail() + "}";

    }
}

# Classe Livraria
package com.mycompany.lojavirtual;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class Livraria {

    public void armazenaLivro(String nomep, double precoUnitario) {

        String sql = "INSERT INTO livraria (nomep, precoUnitario) VALUES ('?','?') ";
        BancodeDados bd = new BancodeDados();

        try {
            Connection conexao = bd.conectar();
            PreparedStatement pstmt = conexao.prepareStatement(sql);
            pstmt.setString(1, nomep);
            pstmt.setDouble(2, precoUnitario);
            pstmt.execute();

            bd.close(pstmt);
            bd.close(conexao);
            System.out.println("Produto adicionado com sucesso");
        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }

    public int buscaLugarLivro(String nomep) {

        String sql = "SELECT * FROM livraria WHERE nome = " + nomep + "";
        BancodeDados bd = new BancodeDados();
        int lugarLivro = 0;

        try {
            Connection conexao = bd.conectar();
            Statement stmt = conexao.createStatement();
            ResultSet result = stmt.executeQuery(sql);

            if (result != null) {
                while (result.next());
                lugarLivro = result.getInt("id");
            }
            bd.close(result);
            bd.close(stmt);
            bd.close(conexao);
            return lugarLivro;

        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage() + "SQL" + e.getSQLState());

        }
        return 0;
    }

    public void removeLivro(String nomep) {
        int posicaoEncontrada = this.buscaLugarLivro(nomep);
        if (posicaoEncontrada == 0) {
            System.out.println("Produto não encontrado na livraria.");
            return;
        }
        String sql = "DELETE FROM livraria WHERE id = ?";
        BancodeDados bd = new BancodeDados();

        try {
            Connection conexao = bd.conectar();
            PreparedStatement pstmt = conexao.prepareStatement(sql);
            pstmt.setInt(1, posicaoEncontrada);
            bd.close(pstmt);
            bd.close(conexao);
            System.out.println("Produto" + posicaoEncontrada + "excluído com sucesso.");
            return;
        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }

    public void imprimeLivro(int index) throws IndexOutOfBoundsException {
        if (index <= 0) {
            System.out.println("Posição informada é inválida. A lista da livraria começa na posição 1");
            return;
        }

        String sql = "SELECT * FROM livraria WHERE id = ?";
        BancodeDados bd = new BancodeDados();

        try {
            boolean achou = false;
            Connection conexao = bd.conectar();
            PreparedStatement pstmt = conexao.prepareStatement(sql);
            pstmt.setInt(1, index);
            ResultSet result = pstmt.executeQuery();

            while (result.next()) {
                achou = true;
                System.out.println("Na posição:" + result.getInt("id") + "temos:");
                System.out.println("Nome:" + result.getString("nome"));
                System.out.println("Preço:" + result.getDouble("preco"));
            }

            if (!achou) {
                System.out.println("Não há produto nessa posição.");
            }
            bd.close(result);
            bd.close(pstmt);
            bd.close(conexao);
            return;

        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }

    public void imprimeLivraria() {
        BancodeDados bd = new BancodeDados();
        String sql = "SELECT * FROM livraria ORDER BY nomep ASC";

        try {
            Connection conexao = bd.conectar();
            Statement stmt = conexao.createStatement();
            ResultSet result = stmt.executeQuery(sql);
            boolean temRegistro = false;

            while (result.next()) {
                temRegistro = true;
                System.out.println("Na posição:" + result.getInt("id") + "temos:");
                System.out.println("Nome:" + result.getString("nome"));
                System.out.println("Idade:" + result.getInt("Idade"));
                System.out.println("Preço:" + result.getDouble("preco"));
                System.out.println("----------------------------------------------");
            }

            if (temRegistro) {
                System.out.println("A lista da livraria está vazia.");
            }
            bd.close(result);
            bd.close(stmt);
            bd.close(conexao);
        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }

    public Livro temLivroNaPosicao(int index) {
        String sql = "SELECT * FROM livraria WHERE id = " + index + "";
        BancodeDados bd = new BancodeDados();
        boolean achou = false;

        try {
            Connection conexao = bd.conectar();
            Statement stmt = conexao.createStatement();
            ResultSet result = stmt.executeQuery(sql);
            Livro livro = null;
            while (result.next()) {
                livro = new Livro(
                        result.getString("nome"),
                        result.getFloat("preco")
                );
                break;
            }
            bd.close(result);
            bd.close(stmt);
            bd.close(conexao);
            return livro;

        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage() + "SQL" + e.getSQLState());

        }
        return null;
    }

    public void atualizaLivraria(int index, String nomep, double precoUnitario) {
        if (this.temLivroNaPosicao(index) == null) {
            System.out.println("Não há livro nessa posição");
            return;
        }
        String sql = "UPDATE cadastro SET nomeC = ?, precoUnitario = ? WHERE id = ?";
        BancodeDados bd = new BancodeDados();
        try {
            Connection conexao = bd.conectar();
            PreparedStatement pstmt = conexao.prepareStatement(sql);
            pstmt.setString(1, nomep);
            pstmt.setDouble(2, precoUnitario);
            pstmt.setInt(3, index);
            pstmt.execute();

            System.out.println("Livraria atualizada com sucesso");
            bd.close(pstmt);
            bd.close(conexao);

        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.err.println(e.getMessage());
        }
    }
}

# Classe Cadastro
package com.mycompany.lojavirtual;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class Cadastro {

    public void armazenaCliente(String nomeC, int idade, String telefone, String email) {
        if (this.buscaPosicaoCliente(nomeC) != 0) {
            System.out.println("Este cliente já foi adicionada na lista da lojinha");
            return;
        }
        String sql = "INSERT INTO cadastro (nomeC, idade, telefone, email) VALUES ('?','?','?','?') ";
        BancodeDados bd = new BancodeDados();

        try {
            Connection conexao = bd.conectar();
            PreparedStatement pstmt = conexao.prepareStatement(sql);
            pstmt.setString(1, nomeC);
            pstmt.setInt(2, idade);
            pstmt.setString(3, telefone);
            pstmt.setString(4, email);
            pstmt.execute();

            bd.close(pstmt);
            bd.close(conexao);
            System.out.println("Cadastro concluído com sucesso");
        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
        return;
    }

    public int buscaPosicaoCliente(String nomeC) {
        String sql = "SELECT * FROM cadastro WHERE nome = " + nomeC + "";
        BancodeDados bd = new BancodeDados();
        int posicaoEncontrada = 0;

        try {
            Connection conexao = bd.conectar();
            Statement stmt = conexao.createStatement();
            ResultSet result = stmt.executeQuery(sql);

            if (result != null) {
                while (result.next());
                posicaoEncontrada = result.getInt("id");
            }
            bd.close(result);
            bd.close(stmt);
            bd.close(conexao);
            return posicaoEncontrada;

        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage() + "SQL" + e.getSQLState());

        }
        return 0;
    }

    public void removeCliente(String nomeC) {
        int posicaoEncontrada = this.buscaPosicaoCliente(nomeC);
        if (posicaoEncontrada == 0) {
            System.out.println("Cliente não encontrado na lista.");
            return;
        }
        String sql = "DELETE FROM cadastro WHERE id = ?";
        BancodeDados bd = new BancodeDados();

        try {
            Connection conexao = bd.conectar();
            PreparedStatement pstmt = conexao.prepareStatement(sql);
            pstmt.setInt(1, posicaoEncontrada);
            bd.close(pstmt);
            bd.close(conexao);
            System.out.println("Cadastro" + posicaoEncontrada + "excluído com sucesso.");
            return;
        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }

    public void imprimeCliente(int index) throws IndexOutOfBoundsException {
        if (index <= 0) {
            System.out.println("Posição informada é inválida. A lista começa na posição 1");
            return;
        }

        String sql = "SELECT * FROM cadastro WHERE id = ?";
        BancodeDados bd = new BancodeDados();

        try {
            boolean achou = false;
            Connection conexao = bd.conectar();
            PreparedStatement pstmt = conexao.prepareStatement(sql);
            pstmt.setInt(1, index);
            ResultSet result = pstmt.executeQuery();

            while (result.next()) {
                achou = true;
                System.out.println("Na posição:" + result.getInt("id") + "temos:");
                System.out.println("Nome:" + result.getString("nome"));
                System.out.println("Idade:" + result.getInt("idade"));
                System.out.println("Altura:" + result.getDouble("altura"));
            }

            if (!achou) {
                System.out.println("Não há registro nessa posição.");
            }

            bd.close(result);
            bd.close(pstmt);
            bd.close(conexao);
            return;

        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
        return;
    }

    public void imprimeClientes() {
        BancodeDados bd = new BancodeDados();
        String sql = "SELECT * FROM cadastro ORDER BY nomeC ASC";

        try {
            Connection conexao = bd.conectar();
            Statement stmt = conexao.createStatement();
            ResultSet result = stmt.executeQuery(sql);
            boolean temRegistro = false;

            while (result.next()) {
                temRegistro = true;
                System.out.println("Na posição:" + result.getInt("id") + "temos:");
                System.out.println("Nome:" + result.getString("nome"));
                System.out.println("Idade:" + result.getInt("Idade"));
                System.out.println("Altura:" + result.getDouble("altura"));
                System.out.println("----------------------------------------------");
            }

            if (temRegistro) {
                System.out.println("A lista de cadastro está vazio.");
            }
            bd.close(result);
            bd.close(stmt);
            bd.close(conexao);
        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }

    public boolean temCadastroNaPosicao(int index) {
        String sql = "SELECT * FROM cadastro WHERE id = " + index + "";
        BancodeDados bd = new BancodeDados();
        boolean achou = false;

        try {
            Connection conexao = bd.conectar();
            Statement stmt = conexao.createStatement();
            ResultSet result = stmt.executeQuery(sql);

            while (result.next()) {
                achou = true;
            }
            bd.close(result);
            bd.close(stmt);
            bd.close(conexao);
            return achou;

        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage() + "SQL" + e.getSQLState());

        }
        return false;
    }

    public void atualizaCadastro(int index, String nomeC, int idade, String telefone, String email) {
        if (!this.temCadastroNaPosicao(index)) {
            System.out.println("Não há cadastro nessa posição");
            return;
        }
        String sql = "UPDATE cadastro SET nomeC = ?, idade = ?, telefone = ?, email = ? WHERE id = ?";
        BancodeDados bd = new BancodeDados();
        try {
            Connection conexao = bd.conectar();
            PreparedStatement pstmt = conexao.prepareStatement(sql);
            pstmt.setString(1, nomeC);
            pstmt.setInt(2, idade);
            pstmt.setString(3, telefone);
            pstmt.setString(4, email);
            pstmt.setInt(5, index);
            pstmt.execute();

            System.out.println("Cadastro atualizado com sucesso");
            bd.close(pstmt);
            bd.close(conexao);

        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.err.println(e.getMessage());
        }
    }
}

# Classe Itens
package com.mycompany.lojavirtual;

public class Itens {

    private Livro livro;
    private double quantidade;

    public Itens(Livro livro, double quantidade) {
        this.livro = livro;
        this.quantidade = quantidade;

    }

    public Livro getLivro() {
        return livro;
    }

    public void setLivro(Livro livro) {
        this.livro = livro;
    }

    public double getQuantidade() {
        return quantidade;
    }

    public void setQuantidade(double quantidade) {
        this.quantidade = quantidade;
    }

    public double calculePrecoTotal() {
        return this.livro.getPrecoUnitario() * this.quantidade;
    }

    public String toString() {
        return "Itens{ Livro =" + livro.toString() + ", quantidade =" + quantidade + '}';
    }

}

# Classe Caixa
package com.mycompany.lojavirtual;

import java.util.ArrayList;

public class Caixa {

    private ArrayList<Itens> nota_fiscal, vendas;

    public Caixa() {

        this.vendas = new ArrayList();

    }

    public boolean isItensEmpty() {
        return this.vendas.isEmpty();
    }

    public boolean isVendaEmpty() {
        return this.vendas.isEmpty();
    }

    public void armazenaProduto(Livro livro, int quantidade) {
        Itens itens = new Itens(livro, quantidade);
        this.vendas.add(itens);
        System.out.println("Livro adicionado ao Caixa com sucesso.");
    }

    public void removeProduto(String nomep) {
        boolean achou = false;

        if (this.isItensEmpty()) {
            System.out.println("O caixa está vazio.");
            return;
        }
        for (int i = 0; i < this.vendas.size(); i++) {

            Itens itens = this.vendas.get(i);

            if (itens.getLivro().getNomeP().equals(nomep)) {
                this.vendas.remove(i);
                achou = true;
                break;
            }
        }
        if (achou == false) {
            System.out.println(nomep + " não foi encontrado no caixa.");
            return;
        }
        System.out.println(nomep + " foi removido do caixa.");
    }

    public void imprimeCaixa() {
        if (this.isItensEmpty()) {
            System.out.println("O caixa está vazio.");
            return;
        }
        int posicao;
        Itens itens;

        double valorTotal = 0;
        for (int i = 0; i < this.vendas.size(); i++) {
            posicao = i + 1;
            itens = this.vendas.get(i);
            valorTotal = valorTotal + itens.calculePrecoTotal();

            System.out.println("Na posição " + posicao + " está " + vendas.toString() + " Preço do Item:" + itens.calculePrecoTotal());
        }

        System.out.println("Valor total da compra: R$ " + valorTotal);
    }

}

# Classe BancodeDados
package com.mycompany.lojavirtual;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class BancodeDados {

    private static String url = "jdbc:mysql://localhost:3306/livraria_Virtual";
    private static String username = "osincriveis";
    private static String password = "sup&r&qu1p&*";

    public Connection conectar() throws ClassNotFoundException, SQLException {

        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection conn = DriverManager.getConnection(url, username, password);

        return conn;
    }

    public void close(Connection conn) throws SQLException {
        conn.close();
    }

    public void close(Statement stmt) throws SQLException {
        stmt.close();
    }

    public void close(PreparedStatement pstmt) throws SQLException {
        pstmt.close();
    }

    public void close(ResultSet result) throws SQLException {
        result.close();
    }

}

# Classe TabelaBD
package com.mycompany.lojavirtual;

import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;

public class TabelaBD {

    public void criarC() {
        String sql = "CREATE TABLE IF NOT EXISTS cadastro("
                + "id_c INT NOT NULLAUTO_INCREMENT,"
                + "nomeC VARCHAR(255) NOT NULL,"
                + "idade INT NOT NULL,"
                + "telefone INT NOT NULL,"
                + "email VARCHAR(255) NOT NULL,"
                + "PRIMARY KEY (id_c))";

        BancodeDados bd = new BancodeDados();

        try {
            Connection conexao = bd.conectar();
            Statement stmt = conexao.createStatement();
            stmt.execute(sql);
            bd.close(stmt);
            bd.close(conexao);
        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }

    public void criarP() {
        String sql = "CREATE TABLE IF NOT EXISTS livraria("
                + "id_p INT NOT NULLAUTO_INCREMENT,"
                + "nomep VARCHAR(255) NOT NULL,"
                + "precoUnitario DOUBLE NOT NULL"
                + "PRIMARY KEY (id_p))";

        BancodeDados bd = new BancodeDados();

        try {
            Connection conexao = bd.conectar();
            Statement stmt = conexao.createStatement();
            stmt.execute(sql);
            bd.close(stmt);
            bd.close(conexao);
        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }
}

# Classe Home
package com.mycompany.lojavirtual;

import java.util.Scanner;

public class Home {

    public static void main(String[] args) {
        //Trabalho bom, trabalho bonito, trabalho bem feito

        String opcaoN, opcaoL;
        Scanner scanner = new Scanner(System.in);
        Cadastro cadastro = new Cadastro();
        Livraria livraria = new Livraria();
        Caixa caixa = new Caixa();
        BancodeDados bancodeDados = new BancodeDados();
        Tabela_BD tbd = new Tabela_BD();

        do {
            System.out.println("__________________________________________________");
            System.out.println("         Olá querido(a) Cliente (o^-^o)");
            System.out.println("  Seja muito bem-vindo(a) a Livraria Virtual.");
            System.out.println(" Aqui tu pode se cadastrar e vender teus livros.   ");
            System.out.println("___________________________________________________");
            System.out.println("|           Digite o que deseja fazer:           |");
            System.out.println("|       1) Cadastrar-se como cliente.            |");
            System.out.println("|       2) Remover login.                        |");
            System.out.println("|       3) Imprimir lista de clientes.           |");
            System.out.println("|       4) Imprimir um cliente da lista.         |");
            System.out.println("|       5) Buscar um cliente.                    |");
            System.out.println("|       6) Adicionar livros.                     |");
            System.out.println("|       7) Comprar livros.                       |");
            System.out.println("|       8) Atualizar cadastro.                   |");
            System.out.println("|       9) Sair do Menu e/ou Venda.             |");
            System.out.println("__________________________________________________");

            opcaoN = scanner.nextLine();

            switch (opcaoN) {

                case "1":
                    System.out.println("Ótimo! faremos o teu cadastro, inicialmente digite teu nome:");
                    String nomeC = scanner.nextLine();

                    System.out.println("Digite a sua idade:");
                    String idade = scanner.nextLine();
                    int idadeTransformada = Integer.parseInt(idade);

                    System.out.println("Digite teu telefone:");
                    String telefone = scanner.nextLine();

                    System.out.println("Digite teu e-mail:");
                    String email = scanner.nextLine();
                    System.out.println("Você foi cadastrado(a) com sucesso.");

                    cadastro.armazenaCliente(nomeC, idadeTransformada, telefone, email);
                    break;

                case "2":
                    System.out.println("Quem deseja remover?");
                    nomeC = scanner.nextLine();
                    cadastro.removeCliente(nomeC);
                    break;

                case "3":
                    cadastro.imprimeClientes();
                    break;

                case "4":
                    int index;

                    System.out.println("Digite a posição na Lista de Login:");
                    String indexString = scanner.nextLine();
                    index = Integer.parseInt(indexString);

                    try {
                        cadastro.imprimeCliente(index);
                    } catch (IndexOutOfBoundsException e) {
                        System.out.println("Número informado não foi encontrado(a) na Lista de Login.");
                    }
                    break;

                case "5":
                    System.out.println("Digite o nome da pessoa a ser procurada: ");
                    nomeC = scanner.nextLine();

                    int posicaoEncontrada = cadastro.buscaPosicaoCliente(nomeC);

                    switch (posicaoEncontrada) {
                        case -1:
                            System.out.println("A agenda está vazia.");
                            break;
                        case 0:
                            System.out.println("Não foi encontrado(a) " + nomeC + " informado na agenda.");
                            break;
                        default:
                            System.out.println("A posição do " + nomeC + " na agenda é: " + posicaoEncontrada);
                    }
                    break;

                case "6":
                    System.out.println("Digite o nome do livro:");
                    String nomeLivro = scanner.nextLine();

                    System.out.println("Digite o preço unitário do livro:");
                    String preco = scanner.nextLine();
                    double precoUnitario = Double.parseDouble(preco);

                    livraria.armazenaLivro(nomeLivro, precoUnitario);
                    break;

                case "7":

                    do {
                        System.out.println("|A) Adicionar Produto.        |");
                        System.out.println("|B) Remover Produto.          |");
                        System.out.println("|C) Mostrar itens do caixa.   |");
                        System.out.println("|E) Encerrar Compra.          |");

                        opcaoL = scanner.nextLine();

                        switch (opcaoL) {

                            case "A":
                                String opcaoSaida;
                                do {
                                    System.out.println("Recomendamos que escolha a opção C para ver ");
                                    System.out.println("todos os itens do caixa antes de finalizar a compra.");
                                    System.out.println("____________________________");
                                    livraria.imprimeLivraria();
                                    System.out.println("Selecione um livro ou a opção 'S' para finalizar");
                                    opcaoSaida = scanner.nextLine();

                                    if (opcaoSaida.equals("S")) {
                                        continue;
                                    }

                                    System.out.println("Informe qual a quantidade desejada:");
                                    String qtd = scanner.nextLine();
                                    int quantidade = Integer.parseInt(qtd);
                                    int lugarLivro = Integer.parseInt(opcaoSaida);

                                    Livro livro = livraria.temLivroNaPosicao(lugarLivro);

                                    if (livro == null) {
                                        System.out.println("Livro não foi encontrado.");
                                        continue;
                                    }

                                    caixa.armazenaProduto(livro, quantidade);
                                    System.out.println("Livro adicionado no caixa.");
                                } while (!"S".equals(opcaoSaida));
                                break;

                            case "B":
                                caixa.imprimeCaixa();
                                System.out.println("______________________________________");
                                System.out.println("Diga o nome do produto para removê-lo:");

                                String nomep = scanner.nextLine();
                                caixa.removeProduto(nomep);
                                break;

                            case "C":
                                caixa.imprimeCaixa();
                                break;
                        }
                    } while (!"E".equals(opcaoL));
                    System.out.println("Compra armazenada.");
                    break;

                case "8":
                    System.out.println("Digite a posição a ser atualizada:");
                    String posicao = scanner.nextLine();
                    index = Integer.parseInt(posicao);
                    System.out.println("Atualizaremos o teu cadastro, inicialmente digite teu nome:");
                    nomeC = scanner.nextLine();

                    System.out.println("Digite a sua idade:");
                    idade = scanner.nextLine();
                    idadeTransformada = Integer.parseInt(idade);

                    System.out.println("Digite teu telefone:");
                    telefone = scanner.nextLine();

                    System.out.println("Digite teu e-mail:");
                    email = scanner.nextLine();

                    System.out.println("Você foi cadastrado(a) com sucesso.");
                    cadastro.atualizaCadastro(index, nomeC, idadeTransformada, telefone, email);

            }

        } while (!"10".equals(opcaoN));
        System.out.println("Menu e/ou venda finalizado(a).");
    }
}

