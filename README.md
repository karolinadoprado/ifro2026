# Documentação de Arquitetura de Rede: Arquitetura Hierárquica Colapsada (Collapsed Core)

## 1. Visão Geral do Projeto

Este repositório contém a documentação técnica, diagramas estruturais em código (Mermaid) e a simulação de infraestrutura para uma rede corporativa baseada no modelo de **Arquitetura Hierárquica Colapsada** (*Collapsed Core Architecture*).

O projeto foi projetado com base em uma planta baixa física corporativa composta por:
- **Área Aberta (*Open Space*):** Composta por múltiplos clusters de estações de trabalho e pontos de acesso sem fio (*Access Points*).
- **Salas Administrativas / Reunião (Setor Norte):** Estações cabeadas e cobertura Wi-Fi dedicada.
- **Salas Individuais / Diretoria (Setor Sul):** Conectividade cabeada de alta disponibilidade e Ponto de Acesso local.

---

## 2. Arquitetura de Rede (*Collapsed Core*)

A topologia adota o modelo colapsado, no qual as camadas de **Core (Núcleo)** e **Distribuição** são fundidas em um único ponto central de alta performance (Switch Layer 3 / Core Switch). Esta abordagem otimiza custos, reduz a latência inter-VLAN e simplifica o gerenciamento sem comprometer a escalabilidade.

```
                    +-----------------------+
                    |    Roteador / Gateway  |
                    +-----------+-----------+
                                |
                    +-----------+-----------+
                    |    Switch Core (L3)   |
                    | (Core + Distribuição) |
                    +----+-----+-----+------+
                         |     |     |
         +---------------+     |     +---------------+
         |                     |                     |
+--------+-------+    +--------+-------+    +--------+-------+
|  Switch Acesso |    |  Switch Acesso |    |  Switch Acesso |
|   (Open Space) |    |  (Admin Norte) |    |   (Admin Sul)  |
+--------+-------+    +--------+-------+    +--------+-------+
         |                     |                     |
   +-----+-----+         +-----+-----+         +-----+-----+
   |           |         |           |         |           |
  PCs         APs       PCs         APs       PCs         APs
```

---

## 3. Diagrama Mermaid (Código)

Para visualizar ou editar o diagrama em ferramentas como **GitHub**, **Notion**, **Excalidraw**, ou **Mermaid Live Editor**, utilize o código abaixo:

```mermaid
graph TD
    classDef router fill:#2c3e50,stroke:#333,stroke-width:2px,color:#fff;
    classDef core fill:#e67e22,stroke:#333,stroke-width:2px,color:#fff;
    classDef accessSwitch fill:#2980b9,stroke:#333,stroke-width:1px,color:#fff;
    classDef ap fill:#27ae60,stroke:#333,stroke-width:1px,color:#fff;
    classDef host fill:#ecf0f1,stroke:#7f8c8d,stroke-width:1px,color:#2c3e50;

    subgraph WAN [Borda da Rede / Internet]
        Router[Roteador Principal / Gateway]:::router
    end

    subgraph CollapsedCore [Camada Colapsada - Core/Distribuição]
        CoreSwitch[Switch Core L3 / Central]:::core
    end

    Router --- CoreSwitch

    subgraph Setor_OpenSpace [Área Open Space]
        Sw_Access_OS1[Switch Acesso OS 1 - Switch0]:::accessSwitch
        Sw_Access_OS2[Switch Acesso OS 2 - Switch1]:::accessSwitch
        Sw_Access_OS3[Switch Acesso OS 3 - Switch2]:::accessSwitch

        AP0[Access Point 0]:::ap
        AP1[Access Point 1]:::ap
        AP2[Access Point 2]:::ap

        PCs_OS1[PC0, PC1, PC2, PC3]:::host
        PCs_OS2[PC4, PC5, PC6, PC7]:::host
        PCs_OS3[PC8, PC9, PC10, PC11]:::host

        Sw_Access_OS1 --- PCs_OS1
        Sw_Access_OS1 --- AP0
        Sw_Access_OS2 --- PCs_OS2
        Sw_Access_OS2 --- AP1
        Sw_Access_OS3 --- PCs_OS3
        Sw_Access_OS3 --- AP2
    end

    subgraph Setor_Admin_Norte [Salas Superiores / Admin Norte]
        Sw_Admin1[Switch Acesso Admin - Switch3]:::accessSwitch
        AP6[Access Point 6]:::ap
        PCs_Admin1[PC12, PC15]:::host
        PCs_WiFi_Norte[PC16 Wireless]:::host

        Sw_Admin1 --- PCs_Admin1
        Sw_Admin1 --- AP6
        AP6 -.- PCs_WiFi_Norte
    end

    subgraph Setor_Admin_Sul [Salas Inferiores / Admin Sul]
        Sw_Admin2[Switch Acesso Sul - Switch7]:::accessSwitch
        AP_Sul[Access Point Sul]:::ap
        PCs_Admin2[PC13, PC14]:::host

        Sw_Admin2 --- PCs_Admin2
        Sw_Admin2 --- AP_Sul
    end

    CoreSwitch === Sw_Access_OS1
    CoreSwitch === Sw_Access_OS2
    CoreSwitch === Sw_Access_OS3
    CoreSwitch === Sw_Admin1
    CoreSwitch === Sw_Admin2
```

---

## 4. Planejamento de Endereçamento IP e VLANs (Exemplo de Tabela)

| ID VLAN | Nome da VLAN | Sub-rede / CIDR | Descrição / Aplicação |
| :--- | :--- | :--- | :--- |
| **VLAN 10** | `ADM_MGMT` | `192.168.10.0/24` | Gerenciamento de Ativos (Switches, Roteadores) |
| **VLAN 20** | `CORP_DATA` | `192.168.20.0/24` | Estações de Trabalho Cabeadas (*Open Space* e Admin) |
| **VLAN 30** | `WIFI_CORP` | `192.168.30.0/24` | Dispositivos Móveis Corporativos via Wi-Fi |
| **VLAN 40** | `GUEST_WIFI` | `192.168.40.0/24` | Rede Visitantes (Isolada) |

---

## 5. Como Utilizar este Repositório

1. **Visualização do Diagrama:** O GitHub renderiza nativamente o bloco de código `mermaid` acima.
2. **Edição do Diagrama:** Copie o código do bloco `mermaid` e cole no [Mermaid Live Editor](https://mermaid.live) para exportar em PNG/SVG ou personalizar a estrutura.
3. **Simulação:** Utilize o arquivo de projeto no Cisco Packet Tracer para testar a conectividade e configurações de VLAN/Trunking.













# Simulação de Ambiente Hierárquico de Rede Local

Relatório prático referente à atividade de simulação de uma rede corporativa estruturada em camadas no **Cisco Packet Tracer**, contemplando escalabilidade, hierarquia, redundância e disponibilidade[cite: 1].

---

## 📋 Descrição do Projeto

Esta atividade consistiu na implementação física e estrutural de um cenário de rede dividido em três camadas principais[cite: 1]:

* **Camada de Núcleo (Core):** Contém um roteador com duas interfaces ligadas a dois switches de núcleo[cite: 1]. Os switches de núcleo possuem quatro conexões paralelas em GigabitEthernet para agregação de links de 4 Gbps[cite: 1].
* **Camada de Distribuição:** Os switches de núcleo foram conectados individualmente a dois switches de distribuição por meio de interfaces de fibra óptica multimodo (com caminhos redundantes em cruzamento) para agregação de links de 2 Gbps[cite: 1].
* **Camada de Borda (Access):** Composta por quatro switches de borda sem recursos de redundância[cite: 1].
* **Dispositivos Finais:** A rede integra quatro computadores desktops, quatro notebooks e um servidor central devidamente cabeados na camada de borda[cite: 1].

---

## 🚀 Testes de Conectividade e Validação 

Para garantir a comunicação lógica de ponta a ponta e validar o bônus da atividade[cite: 1], foi configurado o endereçamento IP estático na sub-rede `192.168.1.0/24` em todos os dispositivos finais. Abaixo estão as evidências e ênfases nos testes de `ping`:

### 1. Comunicação com o Servidor (`192.168.1.100`)
Os testes executados a partir das estações de trabalho e notebooks da borda em direção ao servidor comprovaram que o tráfego de rede atravessa perfeitamente as camadas de acesso, distribuição e núcleo com respostas bem-sucedidas.

> **Print / Evidência do Ping para o Servidor:**
> ```text
> [<img width="678" height="711" alt="Captura de tela 2026-09-27 174054" src="https://github.com/user-attachments/assets/53d1864f-e494-4fb4-9be5-73731b459733" />
<img width="685" height="698" alt="Captura de tela 2026-09-27 174038" src="https://github.com/user-attachments/assets/726c87f2-1b3f-447c-a543-c0930345b6b4" />
<img width="667" height="709" alt="Captura de tela 2026-09-27 173957" src="https://github.com/user-attachments/assets/b52c3a70-d23f-48a3-a9fe-8bca21729208" />
<img width="678" height="694" alt="Captura de tela 2026-09-27 173904" src="https://github.com/user-attachments/assets/0161ad14-214b-44b1-b955-d5f060a9ca82" />
<img width="685" height="672" alt="Captura de tela 2026-09-27 173832" src="https://github.com/user-attachments/assets/3f21af7e-bfd6-4db7-9d17-65f8ba4957c5" />
<img width="689" height="702" alt="Captura de tela 2026-09-27 173709" src="https://github.com/user-attachments/assets/f3aac39c-3048-4a20-a469-05834a7817e5" />
<img width="681" height="704" alt="Captura de tela 2026-09-27 173632" src="https://github.com/user-attachments/assets/da91ea69-ff90-4921-9a8e-d44efaa4bb1d" />
<img width="683" height="706" alt="Captura de tela 2026-09-27 173511" src="https://github.com/user-attachments/assets/6ad197c2-0d2b-4b3b-a1ad-b210c62967cd" />
 ]
> ```

### 2. Comunicação entre Desktops e Notebooks de Lados Opostos
Os pings cruzados realizados entre máquinas conectadas a switches de borda diferentes demonstraram a eficácia do encaminhamento de pacotes em toda a malha hierárquica da topologia.

> **Print / Evidência do Ping entre os Computadores/Laptops:**
> ```text
> [ <img width="680" height="715" alt="Captura de tela 2026-09-27 192419" src="https://github.com/user-attachments/assets/3a22e1e4-9707-4f2b-b6ba-8e9d6a435bfc" />
<img width="683" height="713" alt="Captura de tela 2026-09-27 192355" src="https://github.com/user-attachments/assets/e1f80de4-6f53-44d6-98e5-47fe5f067e8d" />
<img width="681" height="713" alt="Captura de tela 2026-09-27 192338" src="https://github.com/user-attachments/assets/13f9ddd9-0617-4b31-a9b8-9b9d4002d1ae" />
<img width="681" height="717" alt="Captura de tela 2026-09-27 192315" src="https://github.com/user-attachments/assets/aa2f4989-941f-45c5-8cb5-a2693112a2d9" />
<img width="689" height="713" alt="Captura de tela 2026-09-27 192258" src="https://github.com/user-attachments/assets/8ee38254-0545-4f8e-875c-f3080a2c3ceb" />
<img width="689" height="711" alt="Captura de tela 2026-09-27 192233" src="https://github.com/user-attachments/assets/4d678532-f12e-4c20-901c-c9f2b1dad1bd" />
<img width="691" height="713" alt="Captura de tela 2026-09-27 192157" src="https://github.com/user-attachments/assets/feb87fd1-68b3-43e0-a49c-108c0fe5ae90" />
]
> ```

---

## 📁 Arquivos do Projeto
* **`rede_hierarquica.pkt`**

```
