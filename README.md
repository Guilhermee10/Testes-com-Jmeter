# Teste de carga com JMeter - YouTube

Script de teste de carga (Apache JMeter) contra `https://www.youtube.com`, usando massa de dados em CSV.

## Arquivos
| Arquivo | Para que serve |
| --- | --- |
| `youtube-teste.jmx` | Plano de teste do JMeter |
| `dados.csv` | Massa de dados: 10 palavras de busca (uma por linha) |

## Parâmetros do teste
| Item | Valor |
| --- | --- |
| Endpoint | `www.youtube.com` (HTTPS, porta 443) |
| Usuários (threads) | 20 |
| Ramp-up | 60 segundos (entram ~1 usuário a cada 3 s) |
| Duração | 3 minutos (180 s, contando o ramp-up) |
| Massa de dados | `dados.csv`, 10 palavras, reaproveitadas em ciclo |

## O que cada usuário faz (em loop até acabar os 3 minutos)
1. `GET /`: abre a página inicial.
2. `GET /results?search_query=${palavra}`: busca usando a próxima palavra do `dados.csv`.
3. Espera entre 1 e 3 segundos (tempo de pensar) antes de repetir.

Cada requisição tem uma asserção: o código de resposta deve ser **200**.

## Como executar

### Pela interface gráfica
1. Abra o JMeter e use **File → Open** em `youtube-teste.jmx`.
2. Mantenha o `dados.csv` na **mesma pasta** do `.jmx` (o caminho no script é relativo).
3. Clique em ▶ (Start) e acompanhe pelo **Summary Report** e **Aggregate Report**.

### Pela linha de comando (recomendado para carga)
```bash
jmeter -n -t youtube-teste.jmx -l resultados.jtl -e -o relatorio
```
Gera `resultados.jtl` e um relatório HTML na pasta `relatorio/` (abra o `index.html`).
O `relatorio/` precisa não existir antes de rodar, ou estar vazio.

## Observação
Este é um exercício de estudo, com carga baixa e pausas entre as requisições. Não aumente o número de usuários
contra um site de terceiros. O YouTube pode limitar ou bloquear requisições automatizadas (respostas 429/403 ou
página de consentimento), e isso aparece nos relatórios como erros.
