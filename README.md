# FDR_SYSTEM

Aplicação para Windows que diagnostica e ajuda a recuperar um iPhone ligado por USB. Versão **1.5.6**, autor **anymousdark**.

## O que faz

| Página | Internet | Funções |
|---|---|---|
| **Offline** | Não* | Lê todos os dados do aparelho, copiar valores, exportar relatório; painel **Detalhes da bateria** (nível, saúde estimada, ciclos, capacidades, voltagens, carregador e autonomia); **ficha técnica** do modelo (chip, RAM, ecrã, câmara e ano) e aviso de **iOS mais recente**; em recovery/DFU lê modelo, serial, ECID, CPID/BDID e iBoot |
| **Online** | Sim | Consulta o firmware assinado pela Apple, descarrega o IPSW (motor rápido, vários downloads em simultâneo, pausa/retoma) e restaura — deteta o aparelho sozinho, mesmo em recovery/DFU |
| **Restauro** | Não | Progresso do restauro em direto (extração do IPSW + gravação no iPhone) |
| **Reparo** | Não | Verificar modo, entrar/sair do modo recovery, reiniciar, dados em recovery/DFU e guia de DFU; painel de ativação que deteta se o aparelho está associado a uma conta iCloud (sem pedir credenciais), deteção de **gestão empresarial/MDM** (só leitura) e «Pular configuração inicial» |
| **Diagnóstico** | Não | Registo do iOS **em direto** (começar/parar, filtrar e guardar); recolha de **crash logs e panics** para ZIP; validação ou reparação do **emparelhamento**; lista de **perfis de aprovisionamento** — tudo local e só de leitura ou com pedido explícito |
| **Firmware** | Sim | Catálogo de **todos** os iPhones com todas as versões (assinadas e expiradas); lista os downloads em curso e os ficheiros já guardados em `Documentos\iDevice-Firmware` |
| **Acessibilidade** | Não | Fotos e ficheiros como no 3uTools: navegar, importar/exportar, exportar todas as fotos (DCIM) e captura do ecrã do iPhone |
| **Driver** | Não | Instala com um clique os drivers USB que acompanham a aplicação — **Apple (usbaapl64)**, **DFU (Apple oficial, WHQL 538.0.0.0)** e **UsbDk 1.0.22** — com progresso visível e confirmação no fim |
| **Jailbreak** (opcional) | Não | Página própria para a ferramenta **WinRa1n** (checkm8 + checkra1n + palera1n), incluída na aplicação — para iPhones do 5s ao X (chips A7 a A11), iOS 12 a 17 |

\* A página Offline só usa a rede para o aviso de iOS mais recente; a leitura do aparelho e a ficha técnica são offline.

A deteção do aparelho é automática e a aplicação lê sozinha assim que liga o iPhone - não há opção para desligar a leitura. A barra da página Offline reúne os atalhos **Ler iPhone**, **Copiar**, **Copiar tudo**, **Exportar**, **Ver todos** e **Detalhes** numa única linha, com **Revelar IDs** e pesquisa à direita, para dar mais espaço à ficha e à tabela. O registo da aplicação fica **só no ficheiro de logs**; na página **Diagnóstico** pode ver o registo do próprio iPhone em direto. O botão **Exportar logs** gera um ficheiro de diagnóstico pronto a enviar ao suporte.

A janela abre **maximizada à área de trabalho** (sem a margem de 7 px que o modo maximizado deixava) e a barra de estado fica integrada no topo, deixando mais espaço para o conteúdo. A tabela de **Firmware** usa botões de download compactos, com ícone e contorno, e estados em etiquetas verdes/vermelhas, com linhas mais espaçadas e cabeçalhos alinhados. A página **Reparo** tem scroll, para os cartões de ativação e MDM ficarem acessíveis; o cartão de dados de recovery/DFU fica na coluna da direita, debaixo do aviso de DFU. A coluna do aparelho na página Offline também tem scroll quando é precisa. Ao desligar o iPhone, a app limpa os dados da sessão e fecha-se após 5 segundos por privacidade; se o iPhone voltar nesse intervalo, o fecho é cancelado.

O botão **Tema**, no topo, abre o seletor com **cinco temas** — Escuro, Claro, Meia-noite, Floresta e Ametista — e a escolha fica guardada para as próximas aberturas.

## Requisitos

- Windows 10/11 de 64 bits.
- iPhone ligado por USB, desbloqueado; aceitar **Confiar neste computador** quando pedido.
- Os drivers USB vão dentro da aplicação e podem ser instalados na página **Driver** — já não é preciso instalá-los à parte.

## Instalação

1. Descarregue **`FDR_SYSTEM-1.5.6-setup.exe`** na [página de releases](https://github.com/anymousdark/diagnostico-idevice/releases).
2. Execute e siga o assistente (licença, pasta, Menu Iniciar, ícone no ambiente de trabalho). Não precisa de administrador.
3. Para desinstalar, use **Adicionar/Remover Programas**: a desinstalação remove tudo o que a aplicação criou (pastas, ícones, chaves de registo e cache) — os firmwares que descarregou para `Documentos\iDevice-Firmware` ficam guardados, porque são seus.

Quem preferir a versão portátil pode usar o ZIP `FDR_SYSTEM-1.5.6-win-x64.zip` (extrair tudo e executar).

A aplicação verifica **automaticamente se há versões novas** ao abrir e instala a atualização com um clique (botão **Atualizações**, no topo) — ela fecha, instala e **volta a abrir sozinha**. Se o GitHub estiver indisponível ou limitar os pedidos, mostra um aviso em vez de dizer que já tem a versão mais recente.

## Notas

- O restauro **apaga todo o conteúdo** do iPhone e só funciona com firmware que a Apple ainda assina.
- Depois do restauro, a aplicação pode pedir a ativação à Apple automaticamente (opcional).
- Antes de restaurar, a aplicação verifica se o firmware corresponde ao modelo ligado e **recusa** ficheiros de outro modelo.
- O restauro e a saída do modo recovery usam motores **open-source** incluídos na aplicação (projeto libimobiledevice); não é preciso instalar nada à parte.
- A aplicação **nunca** pede Apple ID nem palavra-passe e **não** contorna o bloqueio de ativação (iCloud): se o aparelho tiver uma conta associada, mostra-a mascarada (ex.: `j***@icloud.com`) e explica que só com a conta original é possível ativar. «Pular configuração inicial» funciona apenas em aparelhos **sem** conta associada.
- A página **Reparo** deteta se o aparelho está sob **gestão empresarial (MDM)**: mostra o que o USB expõe e não remove nem contorna a gestão — a remoção faz-se com o administrador do aparelho.
- Os relatórios mascaram serial, ECID, IMEI e MACs, salvo opção em contrário.
- A página **Diagnóstico** usa ferramentas open-source do projeto libimobiledevice incluídas na aplicação: o registo do iOS é só leitura; os crash logs e panics são **copiados** para `Documentos\FDR_SYSTEM\Relatorios` (não são apagados do iPhone) e podem conter dados pessoais — ficam só neste PC e nada é enviado; o emparelhamento só é refeito com pedido explícito e o iPhone desbloqueado; os perfis são apenas listados.
- Os detalhes da bateria vêm do próprio iPhone (como no 3uTools): nível, ciclos, capacidades e voltagens. Fabricante e temperatura só aparecem quando o modelo os expõe — em muitos iPhones esses dois campos não existem no sistema.
- A página **Jailbreak** é um recurso extra e opcional: só faz alguma coisa se a abrires; funciona apenas em iPhones 5s–X (A7–A11) com iOS 12 a 17 e é usada por conta e risco do utilizador, em aparelhos próprios ou autorizados.
- O instalador apresenta o contrato de licença; a aplicação inclui os avisos de terceiros em `THIRD-PARTY-NOTICES.md`. Instalador e aplicação levam **assinatura digital** do autor (a chave pública vai incluída no ZIP).

## Autor

anymousdark
