Markdown
# 🚀 MikroTik Core Router - Grafana HTML Graphics Dashboard

Um dashboard moderno, interativo e estilizado para monitoramento de roteadores e ativos de rede **MikroTik**, construído no Grafana utilizando a extensão **HTML Graphics (ou Custom HTML/JS/CSS)**.

![MikroTik Dashboard](https://raw.githubusercontent.com/michel-ai/Mikrotik/main/preview.png) *(Substitua o link da imagem pelo screenshot do seu repositório se desejar)*

---

## 📋 Sobre o Dashboard

Este painel foi projetado para oferecer uma experiência visual rica (estilo NOC / Cyberpunk Operations Center), combinando a flexibilidade de HTML, CSS e JavaScript customizados com o ecossistema do Grafana.

### 🌟 Destaques do Painel:
* **Indicadores Globais de Performance:** Métricas de CPU, Uso de Memória, Tráfego RX/TX e Saúde do Equipamento.
* **Visão Interativa do Hardware (Chassis/Rack):** Representação gráfica das portas físicas do roteador com alteração dinâmica de status (Link UP / DOWN / Alertas).
* **Mapeamento de Interfaces & Serviços:** Visualização lógica das portas e conexões (Internet, Corporativo, Servidores, Usuários, DMZ e Backup).
* **Grid de Interfaces Físicas:** Status individual em tempo real de portas `ether` e `sfp`.
* **Monitoramento de Tráfego de Rede:** Gráfico de tráfego com picos e oscilações em tempo real.
* **Tabelas de Alertas e VLANs/Bridges:** Tabela centralizadora de incidentes operacionais por nível de severidade (Critical, Warning, Info).

---

## ⚙️ Pré-requisitos

Para que o dashboard funcione corretamente no seu ambiente Grafana, certifique-se de ter os seguintes recursos instalados:

1. **Grafana** (versão 9.0 ou superior recomendada).
2. **Plugin HTML Graphics** (ou plugin similar de HTML/JS customizado para renderização gráfica).
   ```bash
   grafana-cli plugins install nesorode-htmlgraphics-panel
🔄 Como Adaptar de Dados Fictícios para Dados Reais
Atualmente, o código do dashboard vem configurado no modo DEMO (Simulação Ativa), utilizando scripts em JavaScript (ex: Math.random(), intervalos e arrays estáticos) para simular o tráfego e o status das portas físicas.

Siga os passos abaixo para conectar o painel aos seus dados reais da rede:

1. Configurar a Fonte de Dados (Data Source)
Certifique-se de ter uma fonte de dados de métricas atrelada ao seu MikroTik configurada no Grafana. As opções mais comuns são:

Prometheus (com snmp_exporter ou routeros_exporter)

Zabbix / Telegraf (coletando dados via SNMP)

2. Substituir as Consultas (Queries) no Painel
Abra o painel no Grafana e entre no modo Edit (Editar).

Na aba Query (Consultas), remova ou substitua as métricas de simulação e adicione as métricas reais do seu coletor:

CPU: mikrotik_cpu_load ou system_cpu_utilization

Memória: mikrotik_memory_used_percent

Tráfego RX/TX por Porta: ifInOctets / ifOutOctets (convertidos para bits/s)

Status de Link das Portas: ifOperStatus (1 = Up, 2 = Down)

3. Conectar os Dados ao Código HTML/JS Customizado
Se você estiver utilizando as variáveis e bindings do HTML Graphics:

No painel de edição, navegue até a aba de JS Code / Custom Code.

Localize o trecho de simulação (geralmente dentro da função de Update ou Render).

Substitua a geração fictícia de dados pela leitura do objeto data do Grafana:

💡 Exemplo de Adaptação no JavaScript:

JavaScript
// ❌ CÓDIGO ANTERIOR (DADOS FICTÍCIOS / SIMULAÇÃO):
// const cpuUsage = (Math.random() * (30 - 15) + 15).toFixed(1);
// const ether1Status = Math.random() > 0.1 ? 'UP' : 'DOWN';

// ✅ CÓDIGO ADAPTADO (DADOS REAIS DO GRAFANA):
// Captura a última série de dados retornada da Query 'A' (CPU) e Query 'B' (Porta 1)
const seriesCPU = data.series.find(s => s.refId === 'A');
const seriesEther1 = data.series.find(s => s.refId === 'B');

// Pega o último valor numérico
const cpuUsage = seriesCPU ? seriesCPU.fields[1].values.buffer.slice(-1)[0].toFixed(1) : 0;
const ether1Status = seriesEther1 ? (seriesEther1.fields[1].values.buffer.slice(-1)[0] === 1 ? 'UP' : 'DOWN') : 'DOWN';

// Atualiza os elementos do HTML/DOM
document.getElementById('cpu-value').innerText = `${cpuUsage}%`;
document.getElementById('ether1-card').className = ether1Status === 'UP' ? 'status-up' : 'status-down';
4. Ocultar o Banner de Demonstração
No código HTML/CSS do painel, você pode remover ou ocultar o aviso de DEMONSTRAÇÃO - DADOS FICTÍCIOS:

HTML
<!-- Comente ou remova esta div -->
<!-- <div class="badge-demo">A DEMONSTRAÇÃO - DADOS FICTÍCIOS</div> -->
📥 Como Importar o Dashboard
Faça o download do arquivo mikrotik-dashboard.json deste repositório.

No seu Grafana, vá em Dashboards > Import.

Clique em Upload JSON file e selecione o arquivo baixado.

Escolha a sua Data Source e clique em Import.

📄 Licença
Este projeto é de uso livre para personalização e melhorias da comunidade. Sinta-se à vontade para abrir uma Issue ou enviar um Pull Request!
