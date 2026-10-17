# Diagnóstico iDevice

Aplicação para Windows que diagnostica e ajuda a recuperar um iPhone ligado por USB.

## O que faz

| Página | Internet | Funções |
|---|---|---|
| **Offline** | Não | Lê todos os dados do aparelho, copiar valores, exportar relatório; em recovery/DFU lê modelo, serial, ECID, CPID/BDID e iBoot |
| **Online** | Sim | Consulta o firmware assinado pela Apple, descarrega o IPSW (motor rápido, vários downloads em simultâneo, pausa/retoma) e restaura — deteta o aparelho sozinho, mesmo em recovery/DFU |
| **Restauro** | Não | Progresso do restauro em direto (extração do IPSW + gravação no iPhone) |
| **Reparo** | Não | Verificar modo, entrar/sair do modo recovery, reiniciar, dados em recovery/DFU e guia de DFU; painel de ativação que deteta se o aparelho está associado a uma conta iCloud (sem pedir credenciais) e «Pular configuração inicial» |
| **Firmware** | Sim | Catálogo de **todos** os iPhones com todas as versões (assinadas e expiradas) prontas a descarregar |
| **Acessibilidade** | Não | Fotos e ficheiros como no 3uTools: navegar, importar/exportar, exportar todas as fotos (DCIM) e captura do ecrã do iPhone |

A deteção do aparelho é automática. Em caso de erro, o botão **Exportar logs** gera um ficheiro de diagnóstico pronto a enviar ao suporte. Há ainda uma **janela de registo em direto** (ecrã preto tipo consola) que mostra tudo o que a aplicação está a fazer.

## Requisitos

- Windows 10/11 de 64 bits.
- Driver **Apple Mobile Device** instalado.
- iPhone ligado por USB, desbloqueado; aceitar **Confiar neste computador** quando pedido.

## Instalação

1. Descarregue **`Diagnostico-iDevice-<versão>-setup.exe`** na [página de releases](https://github.com/anymousdark/diagnostico-idevice/releases).
2. Execute e siga o assistente (licença, pasta, Menu Iniciar, ícone no ambiente de trabalho). Não precisa de administrador.
3. Para desinstalar, use **Adicionar/Remover Programas**.

Quem preferir a versão portátil pode usar o ZIP `OfflineDeviceDiagnostics-<versão>-win-x64.zip` (extrair tudo e executar).

## Notas

- O restauro **apaga todo o conteúdo** do iPhone e só funciona com firmware que a Apple ainda assina.
- Depois do restauro, a aplicação pode pedir a ativação à Apple automaticamente (opcional).
- Antes de restaurar, a aplicação verifica se o firmware corresponde ao modelo ligado e **recusa** ficheiros de outro modelo.
- O restauro e a saída do modo recovery usam motores **open-source** incluídos na aplicação (projeto libimobiledevice); não é preciso instalar nada à parte.
- A aplicação **nunca** pede Apple ID nem palavra-passe e **não** contorna o bloqueio de ativação (iCloud): se o aparelho tiver uma conta associada, mostra-a mascarada (ex.: `j***@icloud.com`) e explica que só com a conta original é possível ativar. «Pular configuração inicial» funciona apenas em aparelhos **sem** conta associada.
- Os relatórios mascaram serial, ECID, IMEI e MACs, salvo opção em contrário.

## Autor

anymousdark
