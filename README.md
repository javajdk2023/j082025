public class Aplicacao {
    
    private static final String URL = "jdbc:postgresql://localhost:5432/postgres";
    private static final String USUARIO = "postgres";
    private static final String SENHA = "admin";
    
    public static void main(String[] args) {
        Connection conn = iniciarConexao();
        
        if (conn != null) {
            try (Scanner scan = new Scanner(System.in)) {
                
                System.out.println("Digite o cpf do usuario:");
                String cpf = scan.nextLine();
                
                System.out.println("Digite a idade do usuario:");
                String idade = scan.nextLine();
                
                // Inserir no banco de dados
                String sql = "INSERT INTO usuario (cpf, idade) VALUES (?, ?)";
                try (PreparedStatement stmt = conn.prepareStatement(sql)) {
                    stmt.setString(1, cpf);
                    stmt.setString(2, idade);
                    int linhasAfetadas = stmt.executeUpdate();
                    
                    if (linhasAfetadas > 0) {
                        System.out.println("Usuário cadastrado com sucesso!");
                    } else {
                        System.out.println("Nenhum usuário foi cadastrado!");
                    }
                }
            } catch (SQLException e) {
                System.err.println("Erro ao executar operação no banco: " + e.getMessage());
            } finally {
                fecharConexao(conn);
            }
        } else {
            System.out.println("Falha na conexão com o banco de dados!");
        }
    }
    
    public static Connection iniciarConexao() {
        try {
            return DriverManager.getConnection(URL, USUARIO, SENHA);
        } catch (SQLException e) {
            System.err.println("Erro ao conectar ao banco de dados: " + e.getMessage());
            return null;
        }
    }
    
    public static void fecharConexao(Connection conn) {
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                System.err.println("Erro ao fechar conexão: " + e.getMessage());
            }
        }
    }
}
