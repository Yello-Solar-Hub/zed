---
título: Telemetria
descrição: “Quais dados o Zed coleta e como controlar as configurações de telemetria.”
---

# Telemetria no Zed

O Zed coleta dados de telemetria anônimos para compreender os padrões de uso e diagnosticar problemas.

A telemetria se divide em duas categorias:

- **Do lado do cliente**: Métricas de uso e relatórios de falhas. É possível desativá-los nas configurações.
- **Do lado do servidor**: Coletados ao utilizar serviços hospedados, como IA ou Colaboração. Necessários para que esses recursos funcionem.

## Configurando as definições de telemetria

Você tem controle total sobre quais dados são enviados pelo Zed.
Para ativar ou desativar alguns ou todos os tipos de telemetria, abra Configurações ({#kb zed::OpenSettings}) e procure por “telemetria”, ou adicione o seguinte ao seu arquivo de configurações:

```json [settings]
"telemetry": {
    "diagnostics": false,
    "metrics": false
},
```

## Fluxo de dados

Os dados de telemetria são enviados do aplicativo para nossos servidores a cada 5 minutos (ou quando se acumulam 50 eventos) e, em seguida, encaminhados para o serviço apropriado. Atualmente, utilizamos:

- [Sentry](https://sentry.io): Serviço de monitoramento de falhas — armazena eventos de diagnóstico
- [Snowflake](https://snowflake.com): Data warehouse — armazena eventos de diagnóstico e métricas
- [Hex](https://www.hex.tech): Painéis e exploração de dados — acessa dados armazenados no Snowflake
- [Amplitude](https://www.amplitude.com): Painéis e exploração de dados — acessa dados armazenados no Snowflake

## Tipos de telemetria

### Diagnósticos

Os relatórios de falha consistem em um [minidump](https://learn.microsoft.com/en-us/windows/win32/debug/minidump-files) e metadados de depuração. Os relatórios são enviados na próxima vez que o aplicativo for iniciado após uma falha, permitindo que o Zed identifique e corrija os problemas sem que você precise enviar um relatório de bug.

Você pode verificar quais dados são enviados na estrutura `Panic` em [crates/telemetry_events/src/telemetry_events.rs](https://github.com/zed-industries/zed/blob/main/crates/telemetry_events/src/telemetry_events.rs). Veja também: [Depuração de falhas](./development/debugging-crashes.md).

### Métricas do lado do cliente

A telemetria do lado do cliente inclui:

- Extensões dos arquivos abertos
- Recursos e ferramentas utilizadas no editor
- Estatísticas do projeto (por exemplo, número de arquivos)
- Frameworks detectados em seus projetos

Esses dados não incluem seu código nem detalhes confidenciais do projeto. Os eventos são enviados por HTTPS e estão sujeitos a limitação de taxa.

Os dados de uso estão vinculados a um ID de telemetria aleatório. Se você tiver feito a autenticação, esse ID poderá ser associado ao seu e-mail para que a Zed possa analisar padrões ao longo do tempo e entrar em contato para solicitar feedback.

Para verificar o que o Zed relatou, execute {#action zed::OpenTelemetryLog} na paleta de comandos ou clique em `Ajuda > Exibir Log de Telemetria`.

Para obter a lista completa dos tipos de eventos, consulte a enumeração `Event` em [telemetry_events.rs](https://github.com/zed-industries/zed/blob/main/crates/telemetry_events/src/telemetry_events.rs).

### Métricas do lado do servidor

Ao utilizar os serviços hospedados da Zed, coletamos metadados para fins de limitação de taxa e faturamento (por exemplo, uso de tokens). A Zed não armazena suas instruções nem seu código, a menos que você compartilhe feedback explicitamente ou opte por participar da coleta de dados de treinamento do Edit Prediction.

Para obter detalhes sobre os caminhos de solicitação de IA e o compartilhamento opcional de dados, consulte [Privacidade da IA](./ai/privacy-and-security.md) e [Feedback e dados de treinamento](./ai/ai-improvement.md).

## Zed Business

Os administradores do Zed Business podem aplicar uma política de proibição de compartilhamento em toda a organização; os membros não podem optar por participar do [Compartilhamento de dados de treinamento de previsões](./ai/ai-improvement.md#edit-predictions) ou das [Avaliações de feedback de IA](./ai/ai-improvement.md#ai-feedback-with-ratings). Consulte [Compartilhamento de dados](./business/admin-controls.md#data-sharing) em Controles de administração.

<!-- A FAZER: inserir link para o controle de desativação da telemetria em toda a organização assim que estiver disponível (atualmente previsto para uma versão futura) -->

## Dúvidas e perguntas

Se você tiver dúvidas sobre telemetria, pode [abrir um ticket](https://github.com/zed-industries/zed/issues/new/choose) ou enviar um e-mail para hi@zed.dev.
