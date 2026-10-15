---
name: forticlient-vpn-hc
description: 'Guia um usuário (ou agente de IA) pela instalação e configuração do FortiClient VPN para acesso à rede corporativa do Hospital das Clínicas da FMUSP (HC-FMUSP). Cobre download, instalação, configuração do perfil VPN_HC (SSL-VPN, vpn.hc.fm.usp.br:443) e troubleshooting. Use quando alguém precisar conectar à VPN do HC, instalar o FortiClient, ou resolver problemas de acesso remoto aos sistemas do hospital.'
metadata:
  clawdbot:
    emoji: 🔒
---

# FortiClient VPN — HC FMUSP

Skill que guia um agente de IA a facilitar a instalação, configuração e conexão à VPN institucional do Hospital das Clínicas da FMUSP usando o FortiClient.

## When to use

Use esta skill quando o usuário:
- Pedir ajuda para "conectar na VPN do HC"
- Precisar instalar o FortiClient
- Tiver erro de conexão (autenticação, certificado, timeout)
- Quiser configurar em um novo dispositivo
- Mencionar `vpn.hc.fm.usp.br`, `VPN_HC`, ou "acesso remoto HC"

## Decision Tree

1. **Qual o sistema operacional do usuário?** (Windows / macOS / Linux)
   - Se não souber, pergunte. O instalador e os passos diferem.
2. **O FortiClient já está instalado?**
   - Não → **Passo 1: Download e Instalação**
   - Sim → **Passo 2: Configuração do Remote Access**
3. **Já existe o perfil `VPN_HC` configurado?**
   - Não → **Passo 2: Configuração**
   - Sim → **Passo 3: Conexão**
4. **A conexão foi bem-sucedida?**
   - Sim → Recomende testar acesso interno (ex: https://intranet.hc.fm.usp.br)
   - Não → **Troubleshooting**

## Configuração canônica do perfil

| Campo | Valor |
|-------|-------|
| Tipo | SSL-VPN |
| Connection Name | `VPN_HC` |
| Remote Gateway | `vpn.hc.fm.usp.br` |
| Customize Port | `443` |
| Authentication | Prompt on Login |

## Credenciais

- **Usuário:** o user corporativo **sem** o `@hc.fm.usp.br`
  - Exemplo: se o e-mail é `fulano@hc.fm.usp.br`, o user é `fulano`
- **Senha:** a mesma do e-mail corporativo

## Passos guiados

### Passo 1: Download e Instalação

**Para Windows (10/11):**
1. Acesse: https://www.fortinet.com/br/support/product-downloads
2. Clique em **"Somente FortiClient VPN"**
3. Selecione **"FortiClient VPN"** para Windows
4. Baixe o instalador: `FortiClientVPNOnlineInstaller_7.0.exe`
5. Execute o `.exe` baixado
6. Siga o wizard: **Avançar** → **Install** → **Finish**

**Para macOS:**
1. Acesse o mesmo link acima
2. Baixe o instalador para macOS
3. Abra o `.dmg` e arraste o FortiClient para **Aplicativos**

**Para Linux (Debian/Ubuntu):**
1. Baixe o `.deb` do site da Fortinet
2. Instale com: `sudo dpkg -i forticlient_vpn_*.deb`
3. Inicie pelo menu de aplicações ou: `forticlient`

### Passo 2: Configurar Remote Access

1. Abra o FortiClient
2. Clique em **"Configurar a VPN"** (ou "Configure VPN")
3. Selecione **"Remote Access"** como tipo de conexão
4. Preencha os campos:
   - **VPN:** `SSL-VPN`
   - **Connection Name:** `VPN_HC`
   - **Remote Gateway:** `vpn.hc.fm.usp.br`
   - **Customize Port:** `443`
   - **Authentication:** `Prompt on Login`
5. Clique em **"Save"**

### Passo 3: Conectar

1. No FortiClient, selecione o perfil **`VPN_HC`**
2. Clique em **"Conectar"**
3. Quando solicitado, insira:
   - **Usuário:** seu user (sem `@hc.fm.usp.br`)
   - **Senha:** senha do e-mail corporativo
4. Aguarde o status mudar para **"Conectado"** 🟢

## Troubleshooting

### ❌ "Login failed" / "Authentication failed"
- Verifique se o usuário está **sem** o `@hc.fm.usp.br`
- Confirme que a senha é a do **e-mail corporativo** (não a de rede/AD)
- Teste o login em https://mail.hc.fm.usp.br (se funcionar, a credencial está OK)
- Se acabou de trocar a senha, aguarde ~15 min para sincronizar

### ❌ "Unable to establish VPN" / timeout
- Confirme conexão com a internet (tente acessar google.com)
- Teste o gateway: `ping vpn.hc.fm.usp.br` (deve responder)
- Tente a porta alternativa: **`10443`** em vez de `443`
- Desative temporariamente firewall/antivírus para testar
- Se estiver em rede corporativa, pode ter proxy — tente de outra rede (4G do celular)

### ❌ "Certificate error" / "Untrusted certificate"
- Aceite o certificado do Fortinet quando solicitado (cadeia Fortinet/FortiGate)
- Se persistir, reinstale o FortiClient (versão mais recente)
- Verifique a data/hora do sistema (certificados são sensíveis a horário)

### ❌ "Port already in use" / conflito com outra VPN
- Feche outras VPNs ativas (Cisco AnyConnect, WireGuard, OpenVPN, etc.)
- Reinicie o serviço do FortiClient
- No Windows: `net stop forticlient && net start forticlient`

### ❌ VPN conecta mas não acessa sistemas internos
- Verifique se está no perfil correto (`VPN_HC`, não outro)
- Teste: `nslookup intranet.hc.fm.usp.br` (deve resolver)
- Se não resolver, contate o NETI para verificar se sua conta tem permissão

## Recursos

- **Manual oficial:** Cartilha NETI/HC-FMUSP "Manual de Instalação e Configuração do FortiClient", v2.1, 2022
- **Suporte TI/NETI:** helpdesk interno do hospital
- **Downloads Fortinet:** https://www.fortinet.com/br/support/product-downloads
- **Webmail HC (teste de credencial):** https://mail.hc.fm.usp.br

## Notas

- O gateway `vpn.hc.fm.usp.br` é fixo e mantido pelo NETI/HC
- Credenciais são individuais e intransferíveis
- A VPN é para uso institucional; conexões são logadas
- Em caso de dúvida sobre permissões de acesso, contate o NETI diretamente
- Não compartilhe sua senha de e-mail com terceiros
