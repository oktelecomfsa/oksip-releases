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
5. **Sala de conferência e código de captura:** já vêm com `8864` e `*8`. Troque se o seu PABX usar outros, ou apague o que ele não tiver.
6. Salve. A bolinha da conta fica verde quando o ramal registra.

## Iniciar com o Windows

O Oksip já vem marcado para iniciar com o Windows: ao ligar o computador, ele abre na **bandeja**, sem janela, e registra os seus
ramais. Clique no ícone da bandeja para abrir a janela. Para desligar, desmarque em **Ajustes → Inicialização**.

Se você desativar o Oksip nos aplicativos de inicialização do Gerenciador de Tarefas, vale o que está lá: o Windows não abre o
Oksip, mesmo com a opção marcada. No Linux, essa opção ainda não existe.

## A tela inicial

- **Ramal, no topo:** clique em "Ramal" para ver os seus ramais e trocar. Ao trocar, o ramal escolhido registra e **o registro do
  anterior cai**: chamadas, mensagens e o painel de Ramais dele param aqui até você voltar a ele. Se o novo ramal não registrar, a faixa
  de erro avisa, e você pode voltar pela mesma lista. O Oksip reabre no ramal escolhido. Durante uma chamada,
  a troca fica bloqueada.
- **Atalhos:** em "Adicionar", guarde um nome, um número, ramal ou código (como `*700`) e um ícone. **Um clique liga na hora**, pelo
  ramal selecionado. Em "Gerenciar atalhos" você edita, muda a ordem e apaga. Eles ficam só neste computador. Na hora de transferir uma
  chamada, eles também aparecem na lista: um clique preenche o número, e você escolhe entre Consultar e Transferir agora.
- **Microfone e alto-falante:** a tela mostra o que o Oksip identificou e avisa se faltar um deles. O controle muda o volume do Oksip,
  inclusive durante a chamada, sem mexer no volume do sistema nem no toque de chamada. O bipe de outra chamada chegando durante uma
  ligação acompanha o alto-falante: com ele em 0%, o aviso é só na tela. Clique no ícone para silenciar. Para trocar de
  dispositivo, vá em Ajustes → Áudio. Fora de chamada, o microfone fica fechado; na chamada, o medidor mostra o nível de cada lado.

## Atualizações

No Windows e no AppImage, o Oksip procura versão nova ao abrir e a cada 6 horas e baixa em segundo plano. Quando ela
estiver pronta, aparecem uma notificação e o item **Reiniciar para atualizar** na bandeja e em **Ajustes → Sobre**.
**O Oksip nunca reinicia sozinho**: só quando você pede, e nunca no meio de uma chamada (se houver uma, ele espera
ela acabar). Para não procurar sozinho, desligue "Procurar atualizações sozinho" em Ajustes → Sobre; "Procurar
agora" continua valendo.

Fora do seu PABX (e do STUN, se você pôs um), o Oksip só se conecta sozinho a essa procura no GitHub, onde ficam os
instaladores. Ela não leva nada sobre você. O WhatsApp só abre quando você clica em uma tecla de suporte.

## Falar com o suporte

Em **Ajustes → Suporte** ou no menu da bandeja, há duas teclas. As duas abrem o WhatsApp Web no navegador, na conversa
com o suporte da Oktelecom. Se pedir, leia o QR Code para entrar no WhatsApp Web. **Quem envia é você:** a mensagem
aparece pronta, e nada sai sem você clicar em Enviar.

- **Falar com o suporte:** a mensagem já traz o seu ramal e a versão do Oksip.
- **Enviar log do erro:** o Oksip gera um arquivo `suporte-<data>-<hora>.txt`, mostra-o na pasta e abre a conversa com um
  resumo do erro. Arraste o arquivo para a conversa e clique em Enviar.

**O que o arquivo leva:** a versão do Oksip e o sistema, as suas contas (domínio, ramal, transporte, proxy, STUN e estado
do registro), as mensagens de registro das contas ativas, as chamadas com falha ou qualidade ruim (números e nomes, IPs, horários,
estatísticas e mensagens SIP) e os eventos recentes do motor. **Nunca leva** senhas, dígitos digitados durante a chamada, texto de mensagens nem a sua
agenda. É um arquivo de texto: você pode abri-lo e conferir antes de enviar.

Os eventos do motor ficam só na memória: envie o log antes de fechar o Oksip. O arquivo fica em `%APPDATA%\Oksip\logs`
(`~/.config/Oksip/logs` no Linux), e o Oksip guarda só os 5 últimos.

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
| A mensagem fica em "Aceita pelo PABX" e não chega      | O PABX aceitou, mas a entrega é com ele. Teste com outro softphone e, se também falhar, fale com o administrador.      |
| A aba Ramais mostra "Sem informação"                   | O PABX precisa aceitar o monitoramento de ramais (BLF). Peça ao administrador para liberar o `SUBSCRIBE` de `dialog`.                                                                                        |
| "Não há um chaveiro do sistema" (Linux)                | Instale e ative o GNOME Keyring (ou o KWallet, no KDE) e salve a conta de novo.                                                                                                                              |

Para qualquer chamada, **Histórico → a chamada → Detalhes** mostra as estatísticas, a linha do tempo e as mensagens
SIP (sem a senha). É o que o administrador do PABX precisa para investigar.
