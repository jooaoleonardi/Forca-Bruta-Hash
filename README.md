# Forca-Bruta-Hash

Busca por força bruta de uma string a partir de uma hash SHA-256, em Python.

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)

## O problema

Encontrar a string de **exatamente 7 letras minúsculas (`a` a `z`)** cuja hash SHA-256 seja:

```text
70502ff6bb85356055ea52ff0a657afd09a52324a33734ccfb7bdedf05634925
```

Como ponto de partida, a string `aaaaaaa` gera a hash `e46240714b5db3a23eee60479a623efba4d633d27fe4f03c904b9e219a7fbe60`. O espaço de busca vai de `aaaaaaa` até `zzzzzzz`, com 26⁷ = 8.031.810.176 combinações.

## Como funciona

[`breakcode.py`](breakcode.py) divide a busca por **primeira letra**: cada uma das 26 letras vira uma tarefa independente, que testa todas as combinações de 6 letras restantes. As tarefas rodam em paralelo com `multiprocessing.Pool`, usando `cpu_count() - 1` processos, e o primeiro resultado encontrado é devolvido.

| Função | Papel |
| --- | --- |
| `gerar_combinacoes` | Gera, com `itertools.product`, as combinações a partir de um prefixo |
| `encontrar_hash_parcial` | Calcula o SHA-256 de cada combinação e compara com o alvo |
| `encontrar_hash` | Distribui os prefixos entre os processos e reúne os resultados |

## Como rodar

Requer Python 3. Não há dependências externas.

```bash
python3 breakcode.py
```

A busca é pesada: são bilhões de hashes. O tempo depende do número de núcleos da máquina.

## Limitações conhecidas

- Com 1 núcleo, `cpu_count() - 1` resulta em 0 processos e o `Pool` falha.
- As tarefas só terminam quando percorrem todas as combinações do seu prefixo, mesmo depois de outra ter achado a resposta.
- O alvo e o alfabeto ficam fixos no código (`HASH_ALVO` e `CARACTERES`).

## Contexto

Exercício da graduação em Ciência da Computação (UniCEUB), sobre complexidade de busca exaustiva e paralelismo.
