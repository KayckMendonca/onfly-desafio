# Declaração de Uso de Inteligência Artificial

## Ferramenta e Modelo Utilizados
* **Ferramenta:** Google Gemini (Interface Web)
* **Modelo:** Gemini 3 Flash

## Divisão de Responsabilidades

### O que a IA fez:
* Sintetizou e organizou os requisitos técnicos, regras de negócio e critérios de entrega a partir do `README.md` original.
* Revisou os módulos originais e as correções aplicadas para confirmar a aderência às regras do desafio (precisão com `Decimal`, política de status `CONFIRMED`, cálculo de diárias e ordenação do ranking).
* Estruturou o texto e a formatação final dos arquivos de documentação da entrega (`README-ENTREGA.md` e `declaracao.md`).

### O que eu fiz:
* Configurei o ambiente local em Python 3.11, executei a suíte de testes (`pytest`) e analisei os arquivos do repositório.
* Identifiquei e implementei diretamente todas as correções de código nos módulos do domínio (`currency_converter.py`, `status_policy.py`, `stay_calculator.py` e `report_builder.py`).
* Executei a validação com os datasets fornecidos (`exemplo-validacao.csv` e `reservas_hoteis.csv`) e verifiquei a aprovação dos testes automatizados.