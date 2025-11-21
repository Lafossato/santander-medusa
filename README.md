# Ataques de Força Bruta com Medusa e Kali Linux

Projeto desenvolvido como parte do programa **Santander / DIO**, utilizando **Kali Linux**, **Metasploitable 2** e a ferramenta **Medusa** para simulação de ataques de força bruta em ambiente controlado.

---

## 🎯 Objetivos do Projeto

- Compreender ataques de força bruta em diferentes serviços (FTP, Web, SMB);
- Utilizar o Kali Linux e o Medusa para auditoria de segurança em ambiente controlado;
- Documentar o processo técnico de forma clara e reprodutível;
- Reconhecer vulnerabilidades comuns e propor medidas de mitigação;
- Utilizar o GitHub como portfólio técnico.

---

##  Ambiente de Laboratório

- **Atacante:** Kali Linux
- **Alvo:** Metasploitable 2
- **Rede:** VMs configuradas na mesma rede virtual

IPs utilizados no laboratório:

- Kali Linux: `10.0.2.15` (exemplo do ambiente)
- Metasploitable 2: `192.168.245.130`

> Observação: os IPs podem variar de acordo com o ambiente. Ajuste conforme a configuração das suas VMs.

---

##  Varredura Inicial com Nmap

Antes dos ataques, foi realizada uma varredura com **Nmap** para identificar serviços expostos no alvo:

```bash
nmap -sV -O 192.168.245.130
Principais serviços utilizados neste projeto:

21/tcp – FTP

80/tcp – HTTP

445/tcp – SMB

 Screenshot sugerida: images/nmap-scan.png

## Ferramentas Utilizadas

- Kali Linux

- Metasploitable 2

- Medusa

- Nmap

**Cenário 1 – Força Bruta em FTP com Medusa**
Wordlist utilizada
Arquivo wordlist.txt:

```text
Copiar código
123456
password
msfadmin
admin123
qwerty
```

**Comando Medusa**

```bash
medusa -h 192.168.245.130 -u msfadmin -P wordlist.txt -M ftp
-h: host alvo (Metasploitable);

-u: usuário testado (msfadmin);

-P: wordlist com senhas;

-M ftp: módulo FTP.
```

## Resultado

O Medusa encontrou a combinação correta de credenciais:

- Usuário: msfadmin

- Senha: msfadmin

 Screenshot sugerida: images/ftp-medusa.png

Esse teste demonstra como credenciais padrão/fracas podem ser exploradas com facilidade.

## Cenário 2 – Password Spraying em SMB com Medusa
Neste cenário foi utilizada a técnica de password spraying, onde uma única senha é testada em vários usuários.

Lista de usuários
Arquivo users.txt:

```text
Copiar código
msfadmin
user
guest
root
```

#### **Comando Medusa (SMB)**

```bash

medusa -h 192.168.245.130 -U users.txt -p msfadmin -M smbnt
-U: arquivo com lista de usuários;

-p: senha fixa testada para todos;

-M smbnt: módulo SMB.
```

**Resultado**

Foi possível autenticar em conta(s) utilizando a mesma senha em múltiplos usuários, evidenciando o risco de:

- Reutilização de senhas;

- Senhas fracas ou padrão;

- Falta de políticas de complexidade.

Screenshot sugerida: images/smb-medusa.png

**Medidas de Mitigação**

Algumas recomendações para mitigar ataques de força bruta e password spraying:

- Exigir senhas fortes (comprimento mínimo, complexidade, não reutilizar);

- Evitar credenciais padrão (como admin/admin, msfadmin/msfadmin);

- Implementar bloqueio temporário após múltiplas tentativas falhas;

- Habilitar autenticação multifator (MFA);

- Restringir serviços como FTP e SMB por meio de firewalls e VPN;

- Monitorar logs de autenticação e configurar alertas para tentativas suspeitas;

- Promover conscientização sobre boas práticas de senha.

## Lições Aprendidas

Durante este projeto, pude:

- Entender na prática a diferença entre força bruta tradicional e password spraying;

- Utilizar o Medusa contra serviços FTP e SMB em ambiente controlado;

- Perceber como serviços expostos e senhas fracas facilitam o comprometimento;

- Refletir sobre a importância de políticas de segurança e monitoramento;

- Praticar documentação técnica e o uso do GitHub como portfólio.
