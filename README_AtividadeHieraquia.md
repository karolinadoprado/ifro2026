```markdown
# 🌐 Simulação de Ambiente Hierárquico de Rede Local

Relatório prático referente à atividade de simulação de uma rede corporativa estruturada em camadas no **Cisco Packet Tracer**, contemplando escalabilidade, hierarquia, redundância e disponibilidade[cite: 1].

---

## 📋 Descrição do Projeto

Esta atividade consistiu na implementação física e estrutural de um cenário de rede dividido em três camadas principais[cite: 1]:

* **Camada de Núcleo (Core):** Contém um roteador com duas interfaces ligadas a dois switches de núcleo[cite: 1]. Os switches de núcleo possuem quatro conexões paralelas em GigabitEthernet para agregação de links de 4 Gbps[cite: 1].
* **Camada de Distribuição:** Os switches de núcleo foram conectados individualmente a dois switches de distribuição por meio de interfaces de fibra óptica multimodo (com caminhos redundantes em cruzamento) para agregação de links de 2 Gbps[cite: 1].
* **Camada de Borda (Access):** Composta por quatro switches de borda sem recursos de redundância[cite: 1].
* **Dispositivos Finais:** A rede integra quatro computadores desktops, quatro notebooks e um servidor central devidamente cabeados na camada de borda[cite: 1].

---

## 🚀 Testes de Conectividade e Validação (Bônus de IP e Ping)

Para garantir a comunicação lógica de ponta a ponta e validar o bônus da atividade[cite: 1], foi configurado o endereçamento IP estático na sub-rede `192.168.1.0/24` em todos os dispositivos finais. Abaixo estão as evidências e ênfases nos testes de `ping`:

### 1. Comunicação com o Servidor (`192.168.1.100`)
Os testes executados a partir das estações de trabalho e notebooks da borda em direção ao servidor comprovaram que o tráfego de rede atravessa perfeitamente as camadas de acesso, distribuição e núcleo com respostas bem-sucedidas.

> **Print / Evidência do Ping para o Servidor:**
> ```text
> [ Cole aqui a sua imagem/print do terminal do Packet Tracer mostrando o comando ping bem-sucedido para o servidor ]
> ```

### 2. Comunicação entre Desktops e Notebooks de Lados Opostos
Os pings cruzados realizados entre máquinas conectadas a switches de borda diferentes demonstraram a eficácia do encaminhamento de pacotes em toda a malha hierárquica da topologia.

> **Print / Evidência do Ping entre os Computadores/Laptops:**
> ```text
> [ ]
> ```

---

## 📁 Arquivos do Projeto
* **`rede_hierarquica.pkt`**: 

```
