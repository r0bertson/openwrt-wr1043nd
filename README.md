# Instalando OpenWrt no TP-LINK WR1043ND V3

### Introdução

Este tutorial irá guiar você pela troca do firmware original do TP-LINK WR1043ND V3
para OpenWrt com OpenVSwitch 2.7 e suporte a SNMP. É importante perceber que os
arquivos aqui fornecidos são para a versão 3 do modelo em questão e que não há
garantias de que irá funcionar para as demais. Também tenha em mente que trocar o
firmware do aparelho caracteriza violação da garantia do fabricante e que existe
o risco de *brickar* o equipamento. Os arquivos contidos neste tutorial foram obtidos
através fontes confiáveis ([OpenWrt](https://openwrt.org/)/
[TP-LINK](https://www.tp-link.com/)). Caso deseje, pode procurar o
arquivo diretamente lá. Como o modelo WR1043ND foi descontinuado pelo LEDE no
final de 2017, estarei armazenando os arquivos aqui também por medida de segurança.

### Passo a passo

1. Primeiramente conecte o cabo diretamente numa das portas ethernet e entre nas
configurações do roteador pelo navegador através do IP `192.168.1.1`. As credenciais
padrão são admin/admin.

2. Pelo menu, navegue por System Tools -> Firmware Upgrade e selecione o arquivo:
`lede-17.01.4-ar71xx-generic-tl-wr1043nd-v3-squashfs-factory.bin`. Clique para
confirmar e espere. Após algum tempo o roteador irá reiniciar.

3. Agora com o OpenWrt (o projeto foi renomeado para LEDE recentemente) instalado,
 acesse as configurações novamente através do mesmo IP (192.168.1.1), agora com a
  interface LuCI. As credenciais são admin e no campo da senha deixe em branco.

4. Navegando pelo menu System -> Software, verifica-se que entre os pacotes
instalados não constam o OpenVSwitch nem o SNMPd, portanto devemos fazer um
patch com outra imagem.

5. Vá no menu System -> Backup / Flash Firmware e na seção "Flash new firmware
image" selecione o arquivo do patch
`openwrt-ar71xx-generic-tl-wr1043nd-v3-squashfs-sysupgrade.bin`. Após alguns
segundos o roteador irá reiniciar. Perceba que não é mais possível acessar as
configurações do roteador através do navegador. Isso acontece pois após o patch
alguns pacotes foram perdidos, incluindo o LuCI.

6. Agora é necessário acessar o roteador através do SSH. Vá no terminal e digite
`ssh -Y root@192.168.1.1`. Uma tela parecida irá aparecer no seu terminal.
Conecte o cabo do seu modem na porta WAN e atualize a lista de pacotes através
do comando `opkg update`.

7. Em seguida, instale o LuCI digitando `opkg install luci`. Instale também o
SNMPd digitando `opkg install snmpd`.

Pronto, o seu roteador agora conta com o OpenWrt, incluindo OpenVSwitch 2.7.0,
SNMP e LuCI. Se desejar ver quais pacotes estão instalados, digite `opkg
list-installed`. Essa informação também pode ser vista no navegador como no passo 4.

No momento o roteador está operando como um roteador comum.

### Reinstalando firmware original
Se desejar retornar o equipamento para o firmware de fábrica, faça como no passo 5,
porém selecione o arquivo `tplink-1043nd-v3-stripped.bin`. Este arquivo é uma
variação do firmware original encontrado no site do [fabricante](https://www.tp-link.com/br/download/TL-WR1043ND_V3.html#Firmware),
porém sem o menu de boot que pode dar conflito com o OpenWrt. Se desejar, pode
baixar o original e fazer as alterações necessárias para remover o menu de boot
como demonstrado
[aqui](https://wiki.openwrt.org/toh/tp-link/tl-wr1043nd#back_to_original_firmware).

### Problemas comuns

> Não consigo acessar o roteador pelo SSH, o IP não é encontrado.

Configure manualmente a sua interface de rede com o IP `192.168.1.2` e
tente novamente. Lembre-se que o cabo deve estar conectado em uma das portas
lan e não na WAN.

> Mesmo com IP estático, ainda não consigo acessar.

Reinicie o roteador e acione o modo de segurança. Quando a luz do sistema ( * )
começar a piscar, pressione o botão 'WPS/reset' do roteador até ela piscar mais
rápido. Agora tente acesssar novamente.


### Contato

Robertson Lima - [robertsonlima91@gmail.com](mailto:robertsonlima91@gmail.com)
