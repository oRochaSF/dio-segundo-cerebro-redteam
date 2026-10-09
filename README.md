# 🛡️ Segundo Cérebro: Fundamentos Ofensivos & Táticas de Red Team

Repositório dedicado à documentação e estruturação do meu **Segundo Cérebro em Cibersegurança**, desenvolvido no **Google Gemini Notebook (NotebookLM)** para consolidar a transição profissional de infraestrutura e redes para segurança ofensiva (Red Team).

---

## 🎯 1. Tema e Objetivo

> **"Consolidar táticas e técnicas de Red Team para acelerar minha transição de infraestrutura para segurança ofensiva, servindo como base prática para aplicar metodologias ofensivas em ambientes de infraestrutura e labs."**

O propósito deste caderno é servir como um copiloto e mentor técnico diário, permitindo analisar vulnerabilidades a partir da causa-raiz estrutural em protocolos e serviços corporativos (como Active Directory, redes L2/L3 e autenticação Windows), unindo a visão de ataque à prática de mitigação.

---

## 📚 2. Fontes Selecionadas e Justificativa de Confiança

A curadoria foi desenhada seguindo o princípio de *Garbage In, Garbage Out*: apenas materiais autoritativos, perenes e com profundidade técnica e metodológica foram integrados.

### Frameworks Oficiais e Padrões Globais (Texto)
* **MITRE ATT&CK® (Enterprise Matrix):** Base de conhecimento padrão mundial sobre táticas, técnicas e procedimentos (TTPs) de adversários reais. Permite classificar e correlacionar ações ofensivas com eventos de auditoria e segurança.
* **NIST (National Institute of Standards and Technology):** Referência de diretrizes mundiais sobre governança, conformidade, arquitetura segura e mitigação de vulnerabilidades.
* **OWASP Foundation:** A principal comunidade aberta de segurança de software e aplicações web (responsável pelo OWASP Top 10 e pelo Web Security Testing Guide - WSTG).

### Guias Metodológicos e Técnicos (Texto)
* **The Hacker Recipes:** Documentação aberta e aprofundada mantida por pesquisadores ofensivos, com foco detalhado em exploração interna de protocolos corporativos (Active Directory, Kerberos, LDAP, NTLM e sistemas Windows/Linux).
* **Consolidar táticas e técnicas de Red Team... (Documento Interno):** Documento de contexto do projeto com o objetivo de aprendizado e conexões com a área de infraestrutura.

### Raciocínio Investigativo, Labs e Conferências (Vídeo)
* **IppSec (YouTube):** Referência internacional em resolução prática de máquinas (Hack The Box). Seu conteúdo enfatiza o pensamento crítico, a análise aprofundada na enumeração de portas/serviços e a lógica investigativa antes de qualquer exploração.
* **DEF CON Conference (YouTube):** A maior e mais tradicional conferência de segurança ofensiva do mundo, trazendo pesquisas inéditas sobre falhas de protocolos e novas técnicas.
* **Black Hat (YouTube):** Conferência de referência corporativa global com foco em ameaças avançadas e segurança de infraestruturas críticas.
* **Roadsec (YouTube):** Maior evento de segurança e hacking da América Latina, contextualizando a realidade da comunidade e dos desafios de segurança cibernética no Brasil.

---

## ⚙️ 3. Diretriz de Comportamento (Persona & System Prompt)

Para garantir que as respostas do Gemini Notebook fossem precisas, técnicas e alinhadas ao meu momento de carreira, utilizei a estratégia de **Nota de Sistema** (`00 - Diretriz de Comportamento e Persona do Mentor`) como fonte ativa constante:

```text
Você é um mentor sênior de Red Team e Segurança Ofensiva. Minha base é em infraestrutura e redes. Ao responder, sempre explique a causa-raiz da vulnerabilidade, faça a ponte com ambientes corporativos/Active Directory e cite os frameworks oficiais (como MITRE ATT&CK) presentes nas fontes. Seja direto e técnico.
```

### Validação do Comportamento
Ao ser inicializado com a pergunta de apresentação de papel, o caderno prontamente confirmou sua função:
> *"O meu papel neste caderno — Fundamentos Ofensivos: Consolidando Táticas de Red Team — é ajudar você a organizar, analisar e aprofundar seu conhecimento em segurança ofensiva e táticas de Red Team, conectando conceitos de infraestrutura com a prática de cibersegurança."*

---

## 🔍 4. Perguntas Realizadas, Respostas e Fontes Utilizadas

Abaixo estão dois testes práticos realizados no caderno que comprovam a qualidade e profundidade da síntese:

### Pergunta 1: *Como funciona o ataque Man-in-the-Middle (MiTM)?*
* **Resumo da resposta gerada:**
  * **Causa-Raiz:** Falta de autenticação mútua estrita em protocolos legados, broadcast/multicast (LLMNR, NBT-NS) e tráfego em texto claro ou sem assinatura digital (ex.: SMB sem *SMB Signing*).
  * **Ciclo de Ataque:** Mapeou as fases de Interceptação/Poisoning, Manipulação/Captura e Relay.
  * **Cenários de AD & Red Team:** Detalhou o *ARP Cache Poisoning* (MITRE ATT&CK **T1557.002**), envenenamento de LLMNR/NBT-NS (**T1557.001**) para captura de hashes NTLMv2 via *Responder*, e o ataque de **NTLM Relay**.
  * **Mitigações de Infra:** Desativação de LLMNR/NBT-NS via GPO, imposição de *SMB/LDAP Signing* e controles em switch como *Dynamic ARP Inspection (DAI)* e *DHCP Snooping*.
* **Fontes utilizadas pelo Notebook:** MITRE ATT&CK, The Hacker Recipes e IppSec.

---

### Pergunta 2: *Como explorar e prevenir acessos indevidos pelo roubo de MAC em uma rede L2 e L3?*
* **Resumo da resposta gerada:**
  * **Causa-Raiz:** 
    * *Camada 2:* O protocolo Ethernet não autentica a origem do quadro na tabela CAM dos switches.
    * *Camada 3:* O protocolo ARP é *stateless* e aceita respostas não solicitadas (*Gratuitous ARP*).
  * **Vetores Ofensivos:** 
    * Bypass de NAC via clonagem de MAC em dispositivos com MAB (*MAC Authentication Bypass*, como impressoras/telefones IP);
    * Man-in-the-Middle via ARP Poisoning (MITRE ATT&CK **T1557.002**) para captura e relay de credenciais;
    * Ataques de *MAC Flooding* para exaustão de tabela CAM.
  * **Impacto em AD:** Salto de VLANs segregadas para pivoting direto contra Domain Controllers.
  * **Prevenção em Infraestrutura:**
    * *Camada 2:* Implantação de **802.1X (EAP-TLS)**, *Port Security* (sticky MAC), **DHCP Snooping** e **Dynamic ARP Inspection (DAI)**.
    * *Camada 3:* Implementação de **IP Source Guard (IPSG)** e microsegmentação com firewall.
* **Fontes utilizadas pelo Notebook:** The Hacker Recipes, MITRE ATT&CK, IppSec e conferências técnicas (DEF CON / Roadsec).

---

## 🔗 5. Link do Notebook Compartilhado

Acesse o caderno interativo com todas as fontes e notas configuradas:  
👉 [Google NotebookLM - Fundamentos Ofensivos: Consolidando Táticas de Red Team](https://notebook.google.com/notebook/bd485f67-7176-4713-9651-c01c0b87f7d8)