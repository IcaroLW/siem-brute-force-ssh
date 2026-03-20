**Detecção de Brute Force SSH com Wazuh SIEM**  

## Objetivo:
Simular um ataque de força bruta via SSH e validar a capacidade de detecção do SIEM Wazuh em tempo real, demonstrando o fluxo completo desde o ataque até a análise forense do log.

Ambiente:
| Componente | Detalhes |
|---|---|
| **SIEM** | Wazuh Manager — Ubuntu 24.04 LTS (`192.168.56.102`) |
| **Agente alvo** | Ubuntu 24.04 LTS (`192.168.56.101`) |
| **Agente atacante** | Windows 10 (`192.168.56.103`) |
| **Rede** | Host-Only (isolada) — VirtualBox |
| **Ferramenta de ataque** | SSH nativo do Linux |

Técnica Utilizada:
| Campo | Valor |
|---|---|
| **MITRE ATT&CK** | [T1110 — Brute Force](https://attack.mitre.org/techniques/T1110/) |
| **Sub-técnica** | T1110.001 — Password Guessing |
| **Severidade esperada** | 🔴 Alta |

# Execução do Ataque:
O ataque foi executado diretamente no agente Ubuntu, simulando múltiplas tentativas de autenticação SSH com um usuário inexistente:
bashfor i in {1..20}; do ssh usuariofake@localhost; done
O comando gerou 20 tentativas consecutivas de login com o usuário usuariofake, todas resultando em Permission denied.

Evidência do Ataque: 
<img width="1919" height="1079" alt="Permission Denied - Ubuntu" src="https://github.com/user-attachments/assets/c1dacbdc-2eac-4939-9a46-0e3d7c41d483" />


# Detecção pelo Wazuh:
| Rule ID | Descrição | Nível |
|---|---|---|
| **5710** | `sshd: Attempt to login using a non-existent user` | 5 |

O Wazuh registrou 91 eventos relacionados ao ataque no período monitorado.

Evidência — Painel de eventos (Rule 5710):
<img width="1919" height="1017" alt="ouro puro " src="https://github.com/user-attachments/assets/b34417eb-f7b8-4c6e-a819-255ac6dc0095" />


# Análise Forense do Log:

Ao inspecionar um evento individual, o Wazuh expõe os seguintes campos extraídos do log bruto:
| Campo | Valor |
|---|---|
| `@timestamp` | Mar 19, 2026 @ 21:09:55.702 |
| `agent.name` | ubuntu1 |
| `agent.ip` | 192.168.56.101 |
| `data.srcuser` | usuariofake |
| `decoder.name` | sshd |
| `location` | journald |
| `full_log` | `Failed password for invalid user usuariofake from ::1 port 55306 ssh2` |

Evidência no SIEM:

<img width="1918" height="1018" alt="log inteiro" src="https://github.com/user-attachments/assets/c0b3f292-80cb-4f7f-bd9c-335e2d33016e" />


# Vulnerabilidades Identificadas no Agente:

Durante a análise do agente Ubuntu, o módulo de Vulnerability Detection do Wazuh identificou:
| Severidade | Quantidade |
|---|---|
| 🔴 **Critical** | 55 |
| 🟠 **High** | 206 |
| 🟡 **Medium** | 532 |
| 🟢 **Low** | 72 |

Evidência — Painel de Vulnerability Detection:
<img width="1919" height="1016" alt="55cves" src="https://github.com/user-attachments/assets/94b47578-24a1-4619-a56d-71463e093bf0" />


## Conclusão
O Wazuh detectou com sucesso o ataque de brute force SSH em tempo real, correlacionando os eventos individuais de falha de autenticação e registrando cada tentativa com os metadados completos (IP, usuário, porta, timestamp).
Este cenário demonstra:

Capacidade de detecção de ameaças em tempo real
Análise forense de logs de autenticação
Mapeamento para o framework MITRE ATT&CK
Identificação proativa de vulnerabilidades críticas no endpoint


## Referências

Wazuh Documentation
MITRE ATT&CK T1110 — Brute Force
CVE-2023-3326a
