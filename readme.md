# Relatórios de Frequência

Sistema de consultas SQL para geração de relatórios de faltas de alunos em um ambiente educacional integrado com o sistema Sophia.

## Descrição do Projeto

Este repositório contém queries SQL otimizadas para extrair e processar dados de frequência escolar, permitindo análises detalhadas sobre faltas de alunos por segmento educacional, turma e período.

## Arquivos

### Select Principal.sql
Query simples para validar e selecionar turmas pelo código.

**Retorna:**
- `CODTURMA`: Código da turma

**Parâmetros:**
- `:CODTURMA` - Código da turma a ser consultada

### Select Frequencias.sql
Query principal que gera relatório completo de frequência com cálculos de faltas por segmento escolar.

**Retorna:**
| Campo | Descrição |
|-------|-----------|
| `NOME` | Nome do aluno |
| `TURMA` | Nome da turma |
| `CURSO` | Curso do aluno |
| `CODTURMA` | Código da turma |
| `STATUS` | Status da matrícula |
| `SEGMENTO` | Nível educacional (Educação Infantil, Ensino Fundamental, Ensino Médio, etc.) |
| `FALTAS_TOTAIS_EM_TEMPOS` | Total de faltas em tempos de aula |
| `FALTAS_TOTAIS_POR_DIA` | Total de faltas convertidas para dias úteis (conforme segmento) |

**Parâmetros:**
- `:CODTURMA` - Código da turma para filtrar dados

## Segmentos Educacionais

As queries reconhecem os seguintes segmentos:

- **Educação Infantil** (Séries 1-2)
- **Ensino Fundamental I** (Séries 5-9)
- **Ensino Fundamental II** (Séries 10-13)
- **Ensino Médio** (Séries 14-16)
- **Técnico em Administração** (Séries 17-19)
- **Técnico em Enfermagem** (Séries 20-22)
- **Técnico em Informática** (Séries 23-25)
- **Itinerários Formativos** (Séries 26-28)

## Cálculo de Faltas por Dia

O cálculo de faltas convertidas para dias é feito dividindo o total de tempos faltados por um fator específico de cada segmento:

- Segmentos 1-2, 5-9: 4 tempos = 1 dia
- Segmento 10-13: 4.2 tempos = 1 dia
- Segmentos 14-16, 17-19, 20-22, 23-25, 26-28: 6 tempos = 1 dia

## Exemplos de Uso

### Obter frequência de uma turma específica

Selecione a turma, em seguida acesse o contexto Relatórios, selecione personalizado e selecione Relatório de Total de Faltas.

Resultado: Relatório ordenado pelo aluno com a maior quantidade de faltas em tempos de aula, além de também retornar o cálculo de dias de faltas.

## Banco de Dados

- **Tabelas utilizadas**:
  - TURMAS - Contém as turmas
  - MATRICULA - Contém o registro de matrícula de alunos
  - FISICA - Contém o cadastro de pessoas físicas
  - LISTA_CHAMADA_AULA_ALUNO - Contém as faltas da lista de chamada.
  - LISTA_CHAMADA_AULA - Contém informação de matéria lecionada e tarefas.
  - LISTA_CHAMADA - Contém as principais informações de uma Lista de Chamada

## Filtros Aplicados

- Apenas faltas contabilizadas (`LCAA.FALTA = 1`)
- Matrículas ativas (`M.STATUS = '0'`)
- Turma específica (`:CODTURMA`)

## Ordenação

Os resultados são ordenados por:
1. Quantidade de faltas em tempos (decrescente)
2. Nome da turma (crescente)

## Notas

- As queries utilizam `CEILING` para arredondar para cima os dias de falta
- O GROUP BY é feito por aluno, turma e série para evitar duplicação de dados
- As informações de matrículas inativas são filtradas (STATUS = '0')

## Autor

Criado e formatado por Gabriel Sena Gomes

## Licença

Este projeto está disponível para uso de instituições que também adotam o Sistema Sophia Gestão Escolar para uso.
