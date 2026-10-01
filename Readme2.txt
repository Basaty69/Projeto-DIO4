1. Tratamentos que podem ser feitos com segurança
Data original	Tratamento	Resultado
2024/15/07	YYYY/DD/MM → inverter dia/mês	2024-07-15
2025/18/11	YYYY/DD/MM → inverter dia/mês	2025-11-18
2027/15/04	YYYY/DD/MM → inverter dia/mês	2027-04-15

A regra é aplicada somente quando o terceiro componente é um mês válido (01–12) e o segundo componente pode ser um dia válido (01–31). Assim, não fazemos uma inversão arbitrária.

2. Comando/regra que usei

Para datas no formato YYYY/MM/DD, YYYY-MM-DD ou YYYY.MM.DD:

SE a data estiver no formato YYYY-[separador]-A-[separador]-B:

    1. Tentar interpretar como YYYY-MM-DD.

    2. Se for uma data válida:
           retornar YYYY-MM-DD.

    3. Se for inválida E:
           A estiver entre 01 e 31
           E B estiver entre 01 e 12

       então interpretar como YYYY-DD-MM.

    4. Validar novamente a data no calendário.

    5. Se for válida:
           retornar YYYY-MM-DD.

    6. Caso contrário:
           retornar INVALID.

Em forma mais direta:

YYYY-A-B
→ tentar YYYY-MM-DD
→ se inválido e A ∈ [1,31] e B ∈ [1,12]
→ tentar YYYY-B-A
→ se data de calendário válida: corrigir
→ senão: INVALID
3. O que NÃO deve ser corrigido automaticamente

Por exemplo:

2024-10-40

Não pode ser transformado automaticamente em outra data, porque o 40 não pode representar um dia.

Da mesma forma:

2024-02-30
2025/04/31
2025-09-31
2025-12-40
2026-02-30
2026-06-31
2026-07-35
2026-12-38
2027-02-30
2027-06-40
August 32nd, 2027

permanecem:

INVALID

porque não existe uma data de calendário correspondente.

4. Datas com mês escrito

Também mantive a regra:

July 22nd, 2025
→ 2025-07-22

October 12th, 2025
→ 2025-10-12

Jan 8th 2026
→ 2026-01-08

Dec 24th 2026
→ 2026-12-24

E datas como:

April 31st, 2024
November 31st, 2025
February 30th, 2026

continuam INVALID.

5. Um ajuste importante na planilha anterior

Também corrigi um erro da versão V2: datas que o Excel armazenou como data/hora, por exemplo:

2024-04-18 00:00:00

não devem ser consideradas inválidas. Elas são convertidas diretamente para:

2024-04-18

O mesmo vale para os demais valores que o Excel armazenou como datetime.

Resultado final

Dos registros que estavam originalmente como INVALID:

3 foram recuperados com segurança;
21 permanecem INVALID, pois não existe uma correção inequívoca sem inventar ou alterar a informação original.

A nova coluna criada é SaleDateSanitized_Final