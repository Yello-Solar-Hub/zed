---
título: Autenticação com o Zed
descrição: “Faça login no Zed para acessar os recursos de colaboração e os serviços de IA.”
---

# Autentique-se com o Zed

Não é necessário fazer login no Zed. Você pode usar a maioria dos recursos que se espera encontrar em um editor de código sem precisar fazer isso. Apresentaremos aqui os poucos recursos que exigem login e explicaremos como fazê-lo.

## Quais recursos exigem que você faça login?

1. Todos os [recursos de colaboração](./collaboration/overview.md) em tempo real.
2. [Recursos baseados em LLM](./ai/overview.md), caso você esteja usando o Zed como provedor dos seus modelos LLM. Para usar a IA sem precisar fazer login, você pode [trazer e configurar suas próprias chaves de API](./ai/use-api-access.md).

## Entrando

O Zed utiliza o fluxo OAuth do GitHub para autenticar usuários, exigindo apenas o escopo `read:user` do GitHub, que concede acesso somente para leitura às informações do seu perfil no GitHub.

1. Abra o Zed e clique no botão `Sign In`, no canto superior direito da janela, ou execute o comando {#action client::SignIn} na paleta de comandos (`cmd-shift-p` no macOS ou `ctrl-shift-p` no Windows/Linux).
2. Seu navegador padrão será aberto na página de login do Zed.
3. Faça a autenticação com sua conta do GitHub quando solicitado.
4. Após a autenticação bem-sucedida, seu navegador exibirá uma confirmação e você será automaticamente conectado ao Zed.

**Observação**: Se você estiver atrás de um firewall corporativo, certifique-se de que as conexões com `zed.dev` e `collab.zed.dev` estejam permitidas.

## Sair

Para sair do Zed, você pode usar qualquer um destes métodos:

- Clique no ícone de perfil no canto superior direito e selecione `Sair` no menu suspenso.
- Abra a paleta de comandos e execute o comando {#action client::SignOut}.

## Endereços de e-mail {#email}

O endereço de e-mail da sua conta do Zed é aquele fornecido pelo OAuth do GitHub. Se você tiver um endereço de e-mail público, ele será utilizado; caso contrário, será usado o seu endereço de e-mail principal do GitHub. As alterações feitas no seu endereço de e-mail no GitHub podem ser sincronizadas com a sua conta do Zed ao [fazer login no zed.dev](https://zed.dev/sign_in).

O Stripe é utilizado para fins de cobrança e usará o endereço de e-mail da sua conta Zed ao iniciar uma assinatura. Atualmente, as alterações no endereço de e-mail da sua conta Zed não atualizam o endereço de e-mail utilizado no Stripe. Consulte [Atualização das informações de cobrança](./account/billing.md#updating-billing-info) para saber como alterar esse endereço de e-mail.

## Ocultar o botão “Entrar” da interface

Caso o recurso “Entrar” não seja utilizado, é possível ocultá-lo da interface usando a propriedade de configuração `show_sign_in`.
Consulte a [página de personalização visual](./visual-customization.md) para obter mais detalhes.
