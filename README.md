# Write-up: Pentest na Máquina Metasploitable 2

Este é um relatório detalhado do teste de invasão realizado na máquina virtual Metasploitable 2, dentro de um ambiente de laboratório controlado.

**Máquina-Alvo:** Metasploitable 2
**Máquina de Ataque:** Kali Linux
**Objetivo:** Identificar vulnerabilidades, explorá-las para obter acesso inicial e escalar privilégios.

---

## Fase 1: Reconhecimento (Reconnaissance)

O primeiro passo foi identificar as portas e serviços rodando na máquina-alvo. Utilizei a ferramenta `nmap` com os seguintes comandos:

nmap -sV -p- 192.168.56.102

Resultados do Nmap: O scan revelou várias portas abertas. A mais crítica parecia ser a porta 21 (FTP), que está rodando uma versão antiga: vsftpd 2.3.4.

<img width="646" height="517" alt="nmap-scan png" src="https://github.com/user-attachments/assets/41874998-8607-4181-8d77-e6d23cde642f" />
