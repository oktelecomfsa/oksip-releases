# Guia de instalação do Oksip

O Oksip é o softphone da Oktelecom: ele liga direto no PABX da sua empresa, sem servidor no meio. Este guia cobre
o Windows e o Linux. Leva uns 5 minutos.

## Antes de começar

- **Os dados do seu ramal**, que quem administra o PABX passa: o **domínio** (ou o IP) do PABX, o **ramal**
  (usuário) e a **senha**. Às vezes também o transporte (UDP, TCP ou TLS) e a porta.
- **Windows 10 ou 11, 64 bits**, ou **Linux x64** (Ubuntu, Debian, Fedora e parecidos).
- **Microfone e alto-falante**, ou um headset (recomendado: corta o eco).

Os instaladores ficam na [página de Releases do Oksip](https://github.com/oktelecomfsa/oksip-releases/releases). Baixe
só de lá.

## Windows

### 1. Baixar e instalar

1. Baixe o `Oksip-Setup-<versão>.exe`.
2. Abra o arquivo. O instalador não pede senha de administrador: ele instala só para o seu usuário. Você pode
   escolher a pasta; o padrão serve.

**Se aparecer "O Windows protegeu o computador" (SmartScreen):** o Oksip ainda não tem a assinatura digital paga
que faz esse aviso sumir. Clique em **Mais informações** e depois em **Executar assim mesmo**. O aviso aparece para
qualquer programa novo sem assinatura; o instalador é o mesmo que a Oktelecom publicou (a atualização automática
confere a soma de verificação de cada arquivo).

**Se o Windows disser que bloqueou o app ("Controle Inteligente de Aplicativos"):** nesse modo o Windows só deixa
abrir programas assinados, e não há como liberar só o Oksip. Desligar o Controle Inteligente de Aplicativos é uma
decisão de segurança de quem cuida da máquina (fica em Segurança do Windows → Controle de aplicativos e navegador).
Se não puder desligar, o Oksip não roda nessa máquina por enquanto.

### 2. Liberar o firewall

Ao abrir pela primeira vez, o Oksip confere o Firewall do Windows. Se faltar a liberação, ele avisa e oferece
**Liberar**: o Windows pede a confirmação de administrador (num usuário comum, a senha de quem administra a máquina).
Isso cria só uma regra de entrada para o motor SIP do Oksip. Sem ela, chamadas recebidas e o áudio podem falhar.
Se você escolher "Agora não", o Oksip pergunta de novo na próxima vez que abrir.

### 3. Configurar o ramal

Veja [Configurar o ramal](#configurar-o-ramal), abaixo. É igual no Windows e no Linux.

## Linux

### Ubuntu e Debian: o `.deb`

```bash
sudo apt install ./oksip_<versão>_amd64.deb
```

O Oksip aparece no menu de aplicativos (Telefonia) e passa a abrir os links `sip:` e `tel:`. O `.deb` **não se
atualiza sozinho**: para uma versão nova, baixe o `.deb` novo e instale do mesmo jeito.

### Qualquer distro: o AppImage

```bash
chmod +x Oksip-<versão>-x86_64.AppImage
./Oksip-<versão>-x86_64.AppImage
```

O AppImage **se atualiza sozinho**, como no Windows. Se ele não abrir, instale o `libfuse2` (algumas distros não
trazem mais).

### O chaveiro

O Oksip guarda a senha do ramal cifrada no chaveiro do sistema (o GNOME Keyring ou o KWallet). Sem um chaveiro, ele
não salva a senha e avisa. No Ubuntu e no Fedora ele já vem ligado.

### A bandeja

No Ubuntu e no KDE, o ícone do Oksip aparece na bandeja. No GNOME puro (o Fedora, por exemplo) não há bandeja: lá,
fechar a janela minimiza, para o Oksip não sumir de vista.

## Configurar o ramal

1. Abra **Ajustes → Contas → Adicionar conta**.
2. Preencha o **domínio**, o **usuário** (o ramal) e a **senha**. A senha fica cifrada nesta máquina e não sai dela,
   a não ser para o PABX.
3. Escolha o **transporte**:
   - **UDP**, na rede da empresa, é o padrão e o mais simples;
   - **TLS** (porta 5061, se o PABX tiver) quando a rede bloqueia o SIP: **é o caso do 4G de várias operadoras** e
     de redes de hotel. O TLS também passa por roteadores com "SIP ALG" (veja [Problemas comuns](#problemas-comuns));
   - **Automático** tenta o que o PABX anuncia.
4. **Servidor STUN** (opcional): fora da empresa, atrás de um roteador ou no 4G, ele ajuda o áudio a chegar. Use o que
   o administrador indicar ou, por exemplo, `stun.l.google.com:19302`. Na rede da empresa, deixe vazio.
5. Salve. A bolinha da conta fica verde quando o ramal registra.

## Atualizações

No Windows e no AppImage, o Oksip procura versão nova ao abrir e a cada 6 horas e baixa em segundo plano. Quando ela
estiver pronta, aparecem uma notificação e o item **Reiniciar para atualizar** na bandeja e em **Ajustes → Sobre**.
**O Oksip nunca reinicia sozinho**: só quando você pede, e nunca no meio de uma chamada (se houver uma, ele espera
ela acabar). Para não procurar sozinho, desligue "Procurar atualizações sozinho" em Ajustes → Sobre; "Procurar
agora" continua valendo.

A única conexão do Oksip fora do seu PABX (e do STUN, se você pôs um) é essa procura no GitHub, onde ficam os
instaladores. Ela não leva nada sobre você.

## Desinstalar

- **Windows:** Configurações → Aplicativos → Oksip → Desinstalar. Suas contas, o histórico, a agenda e as mensagens
  ficam em `%APPDATA%\Oksip` (para uma reinstalação). Para apagar tudo, apague essa pasta depois.
- **Linux (`.deb`):** `sudo apt remove oksip`. Os dados ficam em `~/.config/Oksip`.
- **Linux (AppImage):** apague o arquivo. Os dados ficam em `~/.config/Oksip`.

## Problemas comuns

| O que acontece                                         | O que fazer                                                                                                                                                                                                  |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| A conta fica em "Registrando…" e não sai               | A rede pode bloquear a porta 5060 (comum no 4G): troque o transporte para **TLS**, porta 5061. Confira também o domínio, o usuário e a senha.                                                                |
| O registro por TCP cai depois de algumas chamadas      | É o "SIP ALG" do roteador estragando as mensagens. Desligue o SIP ALG (ou "SIP Helper", "SIP Passthrough") no roteador, ou use **TLS** ou UDP.                                                               |
| A chamada completa, mas um lado não ouve               | Fora da empresa, ponha um **servidor STUN** na conta. Confira se o firewall foi liberado (Windows). Se continuar, o roteador ou o PABX pode estar no caminho: mande os Detalhes da chamada ao administrador. |
| As chamadas não chegam ou as mensagens recebidas somem | Libere o firewall (Windows) e desligue o SIP ALG do roteador.                                                                                                                                                |
| A aba Ramais mostra "Sem informação"                   | O PABX precisa aceitar o monitoramento de ramais (BLF). Peça ao administrador para liberar o `SUBSCRIBE` de `dialog`.                                                                                        |
| "Não há um chaveiro do sistema" (Linux)                | Instale e ative o GNOME Keyring (ou o KWallet, no KDE) e salve a conta de novo.                                                                                                                              |

Para qualquer chamada, **Histórico → a chamada → Detalhes** mostra as estatísticas, a linha do tempo e as mensagens
SIP (sem a senha). É o que o administrador do PABX precisa para investigar.
