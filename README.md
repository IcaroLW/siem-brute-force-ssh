# 🛡️ Detecção de Brute Force SSH com Wazuh SIEM

[![Security](https://img.shields.io/badge/Security-Blue%20Team-0078d4?style=flat-square&logo=shield)](https://github.com/IcaroLW)
[![MITRE ATT&CK](https://img.shields.io/badge/MITRE-T1110%20Brute%20Force-red?style=flat-square)](https://attack.mitre.org/techniques/T1110/)
[![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-00adef?style=flat-square)](https://wazuh.com)
[![CompTIA Security+](https://img.shields.io/badge/CompTIA-Security%2B%20SY0--701-red?style=flat-square)](https://www.comptia.org/certifications/security)
[![Homelab](https://img.shields.io/badge/Ambiente-Homelab-333?style=flat-square&logo=virtualbox)](https://www.virtualbox.org)

> Simulação controlada de ataque de força bruta via SSH em ambiente isolado, com detecção em tempo real pelo SIEM Wazuh e análise forense dos logs gerados.

---

## 📋 Índice

- [Objetivo](#-objetivo)
- [Ambiente](#-ambiente)
- [Técnica Simulada](#-técnica-simulada)
- [Resultados](#-resultados)
- [Writeup Completo](#-writeup-completo)
- [Lições Aprendidas](#-lições-aprendidas)
- [Próximos Passos](#-próximos-passos)

---

## 🎯 Objetivo

Validar a capacidade de detecção do SIEM Wazuh diante de um ataque de força bruta SSH, demonstrando o fluxo completo:

```
Ataque simulado → Coleta de logs → Correlação de eventos → Alerta → Análise forense
```

---

## 🖥️ Ambiente

| Componente | Sistema | IP | Função |
|---|---|---|---|
| **Wazuh Manager** | Ubuntu Server 24.04 | `192.168.56.102` | SIEM / Gerenciador |
| **Agente alvo** | Ubuntu 24.04 | `192.168.56.101` | Host monitorado |
| **Máquina atacante** | Windows 10 | `192.168.56.103` | Origem do ataque |

> Rede isolada em modo **Host-Only** no VirtualBox — sem tráfego externo.

---

## ⚔️ Técnica Simulada

| Campo | Valor |
|---|---|
| **MITRE ATT&CK** | [T1110 — Brute Force](https://attack.mitre.org/techniques/T1110/) |
| **Sub-técnica** | T1110.001 — Password Guessing |
| **Protocolo** | SSH (porta 22) |
| **Severidade detectada** | 🔴 Nível 10 (Alta) |

---

## 📊 Resultados

| Métrica | Valor |
|---|---|
| Tentativas de login simuladas | 20 |
| Eventos registrados pelo Wazuh | 91 |
| Rule ID disparado | 5710 / 5763 |
| Tempo até primeiro alerta | < 30 segundos |
| CVEs críticos encontrados no agente | 55 |

---

## 📄 Writeup Completo

O writeup detalha passo a passo:
- Execução do ataque com evidências
- Análise dos campos extraídos pelo decoder `sshd`
- Inspeção do evento individual no dashboard Wazuh
- Vulnerabilidades identificadas pelo módulo Vulnerability Detection

👉 **[Acesse o writeup completo →](./writeup.md)**

---

## 💡 Lições Aprendidas

- Configuração de rede em VirtualBox (NAT vs Host-Only) e seu impacto na comunicação entre VMs
- Como o Wazuh correlaciona múltiplos eventos individuais em um único alerta de brute force
- Diferença entre Rule ID 5710 (tentativa única) e 5763 (correlação de múltiplas falhas)
- Importância do campo `data.srcuser` na análise forense de eventos de autenticação

---

## 🔭 Próximos Passos

- [ ] Configurar **Active Response** para bloquear automaticamente IPs após N tentativas
- [ ] Adicionar integração com **VirusTotal** para análise de arquivos suspeitos
- [ ] Implementar regras customizadas mapeadas ao MITRE ATT&CK
- [ ] Simular ataque de **Privilege Escalation** (T1068) e detectar via auditd

---

## 🔗 Referências

- [Wazuh Documentation](https://documentation.wazuh.com)
- [MITRE ATT&CK T1110 — Brute Force](https://attack.mitre.org/techniques/T1110/)
- [CompTIA Security+ SY0-701](https://www.comptia.org/certifications/security)

---

<p align="center">
  <sub>Projeto de homelab para fins educacionais — ambiente isolado e controlado.</sub>
</p>
