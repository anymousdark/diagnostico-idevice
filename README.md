# Diagnóstico iDevice

Aplicação para Windows que diagnostica e ajuda a recuperar um iPhone ligado por USB.

## O que faz

| Página | Internet | Funções |
|---|---|---|
| **Offline** | Não | Lê todos os dados do aparelho, copiar valores, exportar relatório, verificar modo, entrar/sair do modo recovery, reiniciar |
| **Online** | Sim | Consulta o firmware assinado pela Apple, descarrega o IPSW e restaura |

A deteção do aparelho é automática. Em caso de erro, o botão **Exportar logs** gera um ficheiro de diagnóstico pronto a enviar ao suporte.

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
- O restauro e a saída do modo recovery usam motores fornecidos pelo utilizador (ver documentação incluída no instalador).
- Os relatórios mascaram serial, ECID, IMEI e MACs, salvo opção em contrário.

## Autor

anymousdark
