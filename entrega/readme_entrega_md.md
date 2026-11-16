# Relatório de Entrega — Consolidador de Reservas de Hotel (Onfly)

## Defeitos Corrigidos e Causa Raiz

1. **Cálculo de câmbio impreciso e com operação invertida (`src/onfly_report/domain/currency_converter.py`)**
   * **Causa raiz:** A função `convert_to_brl` convertia os valores para `float` (gerando perda de precisão) e dividia o montante pela taxa (`amount / rate`) em vez de multiplicá-los usando `Decimal` (`amount * rate`).

2. **Inclusão indevida de reservas `NO_SHOW` nos totais faturáveis (`src/onfly_report/domain/status_policy.py`)**
   * **Causa raiz:** A função `is_billable` verificava apenas `status != "CANCELLED"`, fazendo com que reservas com status `NO_SHOW` fossem contabilizadas nas receitas e noites ocupadas em vez de filtrar estritamente `status == "CONFIRMED"`.

3. **Cálculo incorreto de diárias na virada de mês ou ano (`src/onfly_report/domain/stay_calculator.py`)**
   * **Causa raiz:** A função `calculate_room_nights` subtraía apenas o atributo numérico do dia (`checkout.day - checkin.day`) em vez de calcular a diferença real entre objetos `date` (`(checkout - checkin).days`).

4. **Ordenação invertida no ranking de hotéis (`src/onfly_report/domain/report_builder.py`)**
   * **Causa raiz:** A ordenação de `ranked_hotels` classificava o valor total (`amount`) em ordem ascendente em vez de decrescente (`-item[1]["amount"]`) antes de aplicar o critério de desempate por `hotel_id`.

## Abordagem e Investigação

1. **Execução inicial da suíte de testes:** Executei `pytest -q` para mapear quais asserções e cenários estavam falhando inicialmente.
2. **Revisão orientada às regras de negócio:** Comparei o comportamento de cada módulo em `src/onfly_report/` contra as especificações do `README.md` (política de status, aritmética de datas civis, precisão monetária com `Decimal` e critérios de ordenação).
3. **Correção isolada e validação:** Apliquei as correções pontuais em cada responsabilidade de domínio, validei o output gerado contra a amostra `data/exemplo-validacao.csv` e confirmei a aprovação integral da suíte de testes sem alterar nenhum arquivo em `tests/`.

## Como Validar

1. Ative o ambiente virtual e instale as dependências de desenvolvimento:
   ```bash
   python3.11 -m venv .venv
   source .venv/bin/activate
   pip install -r requirements-dev.txt
   ```

2. Execute a suíte de testes automatizados:
   ```bash
   pytest -q
   ```

3. Gere a amostra de validação e confira os números com o `README.md`:
   ```bash
   PYTHONPATH=src python -m onfly_report \
     --input data/exemplo-validacao.csv \
     --rates data/taxas_cambio.csv \
     --output output/exemplo.json
   ```

4. Gere o relatório consolidado oficial:
   ```bash
   PYTHONPATH=src python -m onfly_report \
     --input data/reservas_hoteis.csv \
     --rates data/taxas_cambio.csv \
     --output output/report.json
   ```

## Resultado do `pytest`

```text
<COLE AQUI A SAÍDA EXATA DO SEU COMANDO pytest -q>
```