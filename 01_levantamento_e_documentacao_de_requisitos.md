# Levantamento e Documentação de Requisitos
### Requisitos Funcionais
1. **Cadastro de Conta Digital:** O sistema deve permitir que os usuários criem contas digitais com informações básicas (Nome, CPF/CNPJ, e-mail, telefone, data de nascimento, Endereço e senha)
2. **Depósitos:** O usuários deve ser capaz de realizar depósitos em sua conta.
3. **Pagamentos Internos:** O sistema deve permitir que os usuários realizem pagamentos para outras contas digitais dentro da plataforma.
4. **Consulta de Saldo e Extrato:** O sistema deve disponibilizar ao usuário a consulta de saldo e histórico de transações realizadas.
5. **Autenticação de Usuários:** A autenticação deve ser realizada por meioi de integração com o sistema externo já existente (nome fictício: Authen)
6. **Integração com APIs de parceiros Bancários:** O sistema deve processar depósitos e pagamentos utilizando API de bancos parceiros para as operações financeiras e consulta de Extrato (nome fictício: TransPay)

### Requisitos Não Funcionais
1. **Segurança:** Todas as comunicações devem ser criptografadas utilizando HTTPS e OAuth2 para integrações externas. Dados sensíveis devem ser armazenados de forma segura, seguindo boas práticas de segurança como hashing e encriptação
2. **Escalabilidade:** O sistema deve suportar crescimento horizontal para atender a um aumento na quantidade de usuários e transações.
3. **Disponibilidade:** O sistema deve ser altamente disponível, com um SLA mínimo de 95%
4. **Monitoramento e Logs:** Logs detalhados devem ser implementados para auditoria e monitoramento de transações e falhas do sistema.
5. **Performance:** O tempo de resposta para consultas e transações deve ser menor que 1 segundo em condições normais de operação.
6. **Conformidade com a LGPD:** O sistema deve garantir proteção e privacidade de dados, com funcionalidades como consentimento explícito do usuário e anonimização de informações sensíveis, quando necessário.
7. **Portabilidade:** O serviço deve ser containerizado (usando Docker), permitindo fácil implantação em diferentes ambientes.

### Tarefas do Usuário no Sistema
- **Usuários Finais:**
    - Criar conta.]
    - Autenticar-se no sistema.
    - Consultar saldo e extrato.
    - Realizar depósitos.
    - Realizar pagamentos.
- **Administradores:**
    - Monitorar transações.
    - Gerenciar logs e verificar erros.