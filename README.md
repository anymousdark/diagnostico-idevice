# FDR_SYSTEM

**Diagnóstico, recuperação e restauro de iPhone por USB.**

Duas aplicações com o mesmo contrato — offline por defeito, sem credenciais Apple e sem
contornar iCloud, Activation Lock ou MDM:

| Plataforma | Versão | Interface | Pacotes |
|---|---|---|---|
| **Linux x64 / ARM64** | **1.6.5** | Avalonia 11.3 · .NET 8 | `.deb` · AppImage |
| **Windows 10/11 x64** | **1.5.9** | WPF · .NET 8 | instalador · patch · ZIP |

> **Código-fonte privado.** Este repositório publica apenas binários e pacotes.
> As releases Linux usam a tag `linux-vX.Y.Z` e ficam marcadas como **pre-release**,
> para o updater Windows não as apanhar em `/releases/latest`.

## Downloads

| Plataforma | Release | Assets |
|---|---|---|
| **Linux 1.6.5** | [`linux-v1.6.5`](https://github.com/anymousdark/diagnostico-idevice/releases/tag/linux-v1.6.5) | `.deb` amd64, `.deb` arm64, AppImage x86_64, `SHA256SUMS` |
| **Windows 1.5.9** | [`app-v1.5.9`](https://github.com/anymousdark/diagnostico-idevice/releases/tag/app-v1.5.9) | instalador, setup, ZIP, patches delta |

---

## Linux 1.6.5

Port do FDR_SYSTEM para **Linux**, em **Avalonia 11.3 / .NET 8**. Uma janela, **onze
páginas** e as ferramentas oficiais do sistema — `usbmuxd`, `libimobiledevice`,
`ideviceactivation`, `irecovery` e, para restauro, `idevicerestore`.

### Destaques

- **11 páginas**: Offline, Online, Restauro, Reparo, **Ativação**, Diagnóstico, Firmware,
  Acessibilidade (AFC), Driver, **Instalação** e Jailbreak.
- **Ativação só de leitura**: estado por USB, supervisão/MDM, registo de
  autorização/processo e relatório com SHA-256. Não ativa, não contorna o Activation Lock
  e não remove MDM.
- **Instalação pela app**: deteta a distribuição e instala por `pkexec` `usbmuxd`,
  `libimobiledevice-utils`, `ideviceactivation` e `irecovery`/`libirecovery-utils`.
- **Auto-update**: consulta a lista de releases públicas, valida o SHA-256 e aplica a
  AppImage ou instala o `.deb`.
- **Logs por sessão**: cada execução grava tudo em `~/.local/share/FDR-SYSTEM/logs/`.
  No fecho, o log é finalizado, sanitizado e pode ser enviado via `gh` (gist secreto por
  omissão).
- **Sem `sudo` dentro da app** e **228 testes automáticos**.

### Páginas

| Página | Rede | Funções |
|---|---|---|
| **Offline** | Não* | Lê modelo, iOS, serial, ECID, UDID, bateria e armazenamento; tabela completa, ficha técnica, detalhes da bateria, copiar/revelar IDs e exportar JSON. Em recovery/DFU lê modelo, serial, ECID, CPID/BDID e iBoot. |
| **Online** | Sim | Catálogo público `api.ipsw.me`, download segmentado com retoma, vários downloads, verificação SHA-256 e restauro. |
| **Restauro** | Não | Duas barras (preparação/extração e gravação), fase atual, saída do `idevicerestore`, confirmação destrutiva e cancelamento. |
| **Reparo** | Não | Estado do aparelho, entrar/sair de recovery, reiniciar, guia de DFU e deteção de ativação/MDM. |
| **Ativação** | Não | Estado de ativação e MDM, registo de processo/autorização e relatório com SHA-256. Só leitura. |
| **Diagnóstico** | Não | `idevicesyslog` em direto, crash logs para ZIP, `idevicepair` e `ideviceprovision`. |
| **Firmware** | Sim | Catálogo de todos os iPhones, versões assinadas e expiradas, cache de 12 h. |
| **Acessibilidade** | Não | AFC: navegar, importar/exportar, DCIM completo, apagar com confirmação e captura de ecrã. |
| **Driver** | Não | Estado do `usbmuxd`, systemd, udev, grupos e dispositivos; só leitura. |
| **Instalação** | Sim* | Deteta a distribuição e instala dependências por `pkexec`, com registo e verificação final. |
| **Jailbreak** | Não | Deteta `palera1n`/`checkra1n` e mostra os comandos; não executa nem corre `sudo`. |

\* Offline só usa a rede para o aviso de iOS mais recente. Instalação só usa a rede para
atualizar os índices de pacotes.

### Instalação

**Pacote `.deb` (recomendado):**

```bash
# amd64
sudo apt install ./fdr-system_1.6.5_amd64.deb

# arm64
sudo apt install ./fdr-system_1.6.5_arm64.deb
```

Instala em `/opt/fdr-system`, cria o comando `fdr-system`, a entrada no menu e o ícone.
Dependências essenciais: `libimobiledevice-utils`, `ideviceactivation`, `irecovery` (ou
`libirecovery-utils`) e `usbmuxd`. O `idevicerestore` é opcional e só é necessário para
restaurar.

**AppImage (portátil):**

```bash
chmod +x FDR-SYSTEM-1.6.5-x86_64.AppImage
./FDR-SYSTEM-1.6.5-x86_64.AppImage
```

Não precisa do .NET instalado. O acesso ao iPhone continua a precisar de `usbmuxd` e
`libimobiledevice` no sistema.

**Permissões USB:**

```bash
sudo systemctl enable --now usbmuxd
sudo usermod -aG plugdev "$USER"
# termina e volta a entrar na sessão
```

**Verificar:**

```bash
idevice_id -l
ideviceinfo -k ProductType
systemctl status usbmuxd
```

### Atualizações e logs

A app verifica a lista de releases públicas 4 segundos depois de arrancar e pelo botão
**Atualizações**. O download tem progresso, o SHA-256 é comparado com o publicado e, no
fim, a AppImage é substituída atomicamente ou o `.deb` é instalado por
`pkexec apt-get install -y --allow-downgrades`.

Cada sessão cria `app-AAAAMMDD-HHMMSS-PID.log`. No fecho, o log é copiado para
`last-session.log` e sanitizado; se o `gh` estiver autenticado, é enviado para um gist
secreto. Para enviar para um repositório, define `FDR_SYSTEM_LOG_REPO=owner/repo`; para
desativar, `FDR_SYSTEM_LOG_UPLOAD=0`.

> O restauro **apaga todo o conteúdo** e só funciona com firmware que a Apple ainda
> assina. Não remove o bloqueio de ativação (iCloud).

---

## Windows 1.5.9

Aplicação original **C# + WPF** para Windows 10/11 x64. Faz tudo a partir de uma única
janela, com nove páginas, e foi desenhada para funcionar **sem internet sempre que
possível**.

### O que faz

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

\* A página Offline só usa a rede para o aviso de iOS mais recente; a leitura do aparelho
e a ficha técnica são offline.

A deteção do aparelho é automática e a aplicação lê sozinha assim que liga o iPhone — não
há opção para desligar a leitura. A barra da página Offline reúne os atalhos **Ler
iPhone**, **Copiar**, **Copiar tudo**, **Exportar**, **Ver todos** e **Detalhes** numa
única linha, com **Revelar IDs** e pesquisa à direita. O registo da aplicação fica **só no
ficheiro de logs**; na página **Diagnóstico** pode ver o registo do próprio iPhone em
direto. O botão **Exportar logs** gera um ficheiro de diagnóstico pronto a enviar ao
suporte.

A janela abre **maximizada à área de trabalho** e a barra de estado fica integrada no
topo. A tabela de **Firmware** usa botões compactos, com ícone e contorno, e estados em
etiquetas verdes/vermelhas. A página **Reparo** tem scroll, para os cartões de ativação e
MDM ficarem acessíveis. Ao desligar o iPhone, a app limpa os dados da sessão e fecha-se
após 5 segundos por privacidade; se o iPhone voltar nesse intervalo, o fecho é cancelado.

O botão **Tema**, no topo, abre o seletor com **cinco temas** — Escuro, Claro, Meia-noite,
Floresta e Ametista — e a escolha fica guardada.

### Novidades da 1.5.9

- **Updater mais seguro:** verificação SHA-256 do executável de origem, cópia de segurança
  `FDR_SYSTEM.exe.old`, rollback automático se a versão nova não arrancar em 15 segundos e
  fallback para o instalador completo.
- **Bateria mais transparente:** valor bruto e arredondado, botão **Medir 3×** com mediana
  e limiar explícito (Boa ≥90%, Razoável ≥80%, Fraca <80%).
- **Leitura mais rápida:** espaço, `AppleSmartBattery` e `com.apple.mobile.battery` são
  consultados em paralelo depois da leitura principal.
- **Logs rodados:** ficheiros com mais de 14 dias são removidos no arranque.
- **Firmware:** o catálogo continua a ser carregado apenas quando a página Firmware é
  aberta.

### Requisitos

- Windows 10/11 de 64 bits.
- iPhone ligado por USB, desbloqueado; aceitar **Confiar neste computador** quando pedido.
- Os drivers USB vão dentro da aplicação e podem ser instalados na página **Driver** — já
  não é preciso instalá-los à parte.

### Instalação

1. Descarregue **`FDR_SYSTEM-1.5.9-installer.exe`** na [página de
   releases](https://github.com/anymousdark/diagnostico-idevice/releases) para uma
   instalação nova ou reparação.
2. Execute e siga o assistente (licença, pasta, Menu Iniciar, ícone no ambiente de
   trabalho). Não precisa de administrador.
3. Para desinstalar, use **Adicionar/Remover Programas**: a desinstalação remove tudo o
   que a aplicação criou — os firmwares que descarregou para
   `Documentos\iDevice-Firmware` ficam guardados, porque são seus.

Quem já tem a **1.5.7 ou 1.5.8** pode usar o atualizador pequeno
`FDR_SYSTEM-1.5.9-setup.exe`. Quem preferir a versão portátil pode usar o ZIP
`FDR_SYSTEM-1.5.9-win-x64.zip` (extrair tudo e executar).

A aplicação verifica **automaticamente se há versões novas** ao abrir e instala a
atualização com um clique (botão **Atualizações**, no topo) — ela fecha, instala e **volta
a abrir sozinha**. A 1.5.9 usa um patch delta de cerca de 6 MB, verifica o SHA-256 do
executável de origem, guarda uma cópia de segurança e repõe a versão anterior se a nova
não arrancar em 15 segundos. Se o patch falhar, usa o instalador completo. Se o GitHub
estiver indisponível ou limitar os pedidos, mostra um aviso em vez de dizer que já tem a
versão mais recente.

### Notas

- O restauro **apaga todo o conteúdo** do iPhone e só funciona com firmware que a Apple
  ainda assina.
- Depois do restauro, a aplicação pode pedir a ativação à Apple automaticamente (opcional).
- Antes de restaurar, a aplicação verifica se o firmware corresponde ao modelo ligado e
  **recusa** ficheiros de outro modelo.
- O restauro e a saída do modo recovery usam motores **open-source** incluídos na
  aplicação (projeto libimobiledevice); não é preciso instalar nada à parte.
- A aplicação **nunca** pede Apple ID nem palavra-passe e **não** contorna o bloqueio de
  ativação (iCloud): se o aparelho tiver uma conta associada, mostra-a mascarada
  (ex.: `j***@icloud.com`) e explica que só com a conta original é possível ativar.
  «Pular configuração inicial» funciona apenas em aparelhos **sem** conta associada.
- A página **Reparo** deteta se o aparelho está sob **gestão empresarial (MDM)**: mostra o
  que o USB expõe e não remove nem contorna a gestão — a remoção faz-se com o
  administrador do aparelho.
- Os relatórios mascaram serial, ECID, IMEI e MACs, salvo opção em contrário.
- A página **Diagnóstico** usa ferramentas open-source do projeto libimobiledevice
  incluídas na aplicação: o registo do iOS é só leitura; os crash logs e panics são
  **copiados** para `Documentos\FDR_SYSTEM\Relatorios` (não são apagados do iPhone) e
  podem conter dados pessoais — ficam só neste PC e nada é enviado; o emparelhamento só é
  refeito com pedido explícito e o iPhone desbloqueado; os perfis são apenas listados.
- Os detalhes da bateria vêm do próprio iPhone (como no 3uTools): nível, ciclos,
  capacidades e voltagens. Fabricante e temperatura só aparecem quando o modelo os expõe.
- A página **Jailbreak** é um recurso extra e opcional: só faz alguma coisa se a abrires;
  funciona apenas em iPhones 5s–X (A7–A11) com iOS 12 a 17 e é usada por conta e risco do
  utilizador, em aparelhos próprios ou autorizados.
- O instalador apresenta o contrato de licença; a aplicação inclui os avisos de terceiros
  em `THIRD-PARTY-NOTICES.md`. Instalador e aplicação levam **assinatura digital** do
  autor (a chave pública vai incluída no ZIP).

---

## Segurança e limites

- Nunca pedir Apple ID, palavra-passe ou código de verificação.
- Não contornar iCloud, Activation Lock ou MDM. A app pode detetar o estado, mas não
  remove bloqueios.
- Não executar ações destrutivas sem confirmação explícita.
- Mascarar UDID, IMEI, série e outros identificadores por omissão.
- Não enviar dados do dispositivo para servidores próprios.
- No Linux, não correr `sudo` dentro da aplicação; a elevação é explícita por `pkexec`.

## Autor

**anymousdark** · Todos os direitos reservados.

O FDR_SYSTEM não é afiliado à Apple. iPhone, iOS, iCloud e Apple Devices são marcas da
Apple Inc. As ferramentas de terceiros mantêm as suas próprias licenças.
