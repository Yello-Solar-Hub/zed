---
título: Funções - Zed
descrição: Compreender as funções organizacionais do Zed e o que cada função pode acessar, gerenciar e configurar.
---

# Funções

A cada membro de uma organização Zed é atribuída uma função que determina o que ele
podem acessar e configurar.

## Tipos de funções {#roles}

A cada membro de uma organização é atribuída uma das quatro funções:

| Capacidade                                                      | Proprietário | Admin | Gerente de Faturamento | Membro |
| --------------------------------------------------------------- | ----- | ----- | --------------- | ------ |
| Utilize IA hospedada e edite previsões por meio do Business             | Sim   | Sim   | Não              | Sim    |
| Ver membros da organização                                       | Sim   | Sim   | Sim             | Sim    |
| Convidar membros                                                  | Sim   | Sim   | Não              | Não     |
| Alterar funções de membros que não são proprietários                                   | Sim   | Sim   | Não              | Não     |
| Remover membros que não sejam proprietários                                        | Sim   | Sim   | Não              | Não     |
| Configurar as definições da organização e os controles de dados               | Sim   | Sim   | Não              | Não     |
| Visualizar informações sobre assinatura, uso e cobrança               | Sim   | Sim   | Sim             | Não     |
| Atualizar dados de cobrança, informações de identificação fiscal e formas de pagamento | Sim   | Sim   | Sim             | Não     |
| Cancelar a assinatura                                         | Sim   | Não    | Não              | Não     |
| Transferir a propriedade                                              | Sim   | Não    | Não              | Não     |

### Proprietário {#role-owner}

Um proprietário tem controle total sobre a organização, incluindo:

- Convidar e remover membros
- Atribuir e alterar funções dos membros
- Gerenciar cobranças, formas de pagamento e notas fiscais
- Configurar políticas de compartilhamento de dados
- Desativar os recursos colaborativos do Zed
- Controle se os membros podem usar modelos hospedados no Zed e as previsões de edição do Zed
- Transferir a propriedade para outro membro

### Administrador {#role-admin}

Os administradores podem gerenciar membros, funções, configurações da organização, controles de dados e
faturamento. Eles têm os mesmos direitos que o Proprietário, exceto que não podem:

- Cancelar a assinatura
- Transferir a propriedade da organização

Essa função é indicada para líderes de equipe ou gerentes que lidam com o dia a dia
configurações de acesso dos membros e da organização.

### Gerente de Faturamento {#role-billing-manager}

Os gerentes de cobrança podem visualizar o uso da assinatura, atualizar os dados de cobrança e o número de identificação fiscal
obter informações, atualizar formas de pagamento e acessar o histórico de faturas.

Essa função não é contabilizada nas licenças pagas do plano Business. Além disso, ela também não inclui
Modelos de IA hospedados no Zed ou Editar previsões por meio da assinatura Business.
Os gerentes de faturamento não podem convidar ou remover membros, alterar funções dos membros, configurar
configurações da organização ou controles de dados, cancelar a assinatura ou transferir
propriedade.

### Membro {#role-member}

Os membros têm acesso padrão ao Zed por meio da assinatura Business. Eles
não é possível acessar as configurações de cobrança ou da organização.

## Gerenciamento de funções de usuário {#managing-users}

Os proprietários e administradores podem gerenciar os membros da organização a partir do painel do Zed, dentro de
a página “Membros”.

### Convidando membros {#inviting-members}

1. Na página “Membros”, selecione **+ Convidar membro**.
2. Insira o endereço de e-mail corporativo do membro e selecione uma função.
3. O convidado recebe um e-mail com instruções para participar. Depois de
   Ao aceitar, eles fazem a autenticação pelo GitHub.

### Alterando a função de um membro {#changing-roles}

1. Na página “Membros”, localize o membro. Você pode filtrar por função ou
   pesquisar por nome.
2. Abra o menu com os três pontos e selecione uma nova função.

### Excluindo um membro {#removing-members}

1. Na página “Membros”, localize o membro.
2. Selecione **Remover** e confirme.

Ao remover um membro, você retira o acesso dele às configurações da organização e a quaisquer recursos gerenciados pela organização. Ele poderá continuar usando o Zed por conta própria.
