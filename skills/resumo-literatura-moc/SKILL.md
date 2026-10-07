---
name: resumo-literatura-moc
description: Resume um conjunto de artigos científicos (PDFs ou textos colados) em notas individuais no formato Obsidian e gera uma Nota Master (Map of Content - MoC) que indexa e sintetiza a pasta inteira, com padrões, conceitos interconectados e lacunas de pesquisa. Use sempre que o usuário enviar vários artigos, papers ou textos de PDFs e pedir revisão de literatura, síntese, fichamento, resumo com MoC, notas interligadas ou "resuma esses artigos e crie um índice", mesmo que não mencione Obsidian nem MoC.
---

# Resumo de literatura com Nota Master (MoC)

Gera duas coisas a partir de um conjunto de artigos:

1. **Parte 1:** uma nota individual por artigo.
2. **Parte 2:** uma Nota Master (MoC) que indexa as notas e sintetiza os padrões entre elas.

## Fluxo

1. **Obter os textos.**
   - Se o usuário colou os textos (separados por `--- ARTIGO 1 ---`, `--- ARTIGO 2 ---` etc.), use-os diretamente.
   - Se enviou PDFs, liste `/mnt/user-data/uploads` e extraia o texto com `pdftotext -layout arquivo.pdf -` (alternativa: `pdfplumber` ou `pypdf`). Se o PDF for escaneado, avise e use OCR (`pytesseract`) se disponível.
2. **Ler cada artigo por inteiro** (objetivo, método, resultados, discussão, limitações), não só o abstract.
3. **Escrever a nota individual** de cada artigo (modelo abaixo) e salvar em `/mnt/user-data/outputs/`.
4. **Só depois de todas as notas prontas**, escrever a Nota Master (modelo abaixo), usando os títulos curtos exatos das notas para que os `[[links]]` funcionem.
5. **Apresentar os arquivos** com `present_files` (nota master primeiro) e encerrar com uma frase curta informando quantos artigos foram processados e quais, se houver, falharam.

## Parte 1: Nota individual

Nome do arquivo = `<Título Curto do Artigo>.md` (o mesmo texto usado nos `[[links]]` da MoC). Título curto: até ~6 palavras, sem caracteres proibidos em nomes de arquivo (`/ \ : * ? " < > |`).

```markdown
---
tags: [literatura, artigo]
autores: [<autores>]
ano: <ano>
veiculo: <revista, conferência ou instituição>
data_criacao: <data de hoje, AAAA-MM-DD>
---
# <Título Curto do Artigo>

**Título completo:** <título original>

## 🎯 Objetivo
<Problema abordado e o que o artigo pretende alcançar. 2 a 4 frases.>

## 🛠️ Método
<Dados, modelo, experimento ou abordagem teórica. 3 a 6 frases.>

## 📊 Resultados
<Principais achados, com números e métricas quando existirem.>

## ✅ Conclusão
<O que os autores concluem e implicações.>

## 💡 Notas e Insights
- **Limitações apontadas:** [Resumo das restrições do modelo ou escopo apontadas pelos autores]
- **Ideia de conexão:** [Como este artigo se liga ao panorama geral da pesquisa em arquiteturas e ecossistemas complexos?]
```

> Observação: o prompt original do usuário trazia apenas o final do modelo da Parte 1 (a seção "Notas e Insights"). As demais seções acima são o padrão adotado; se o usuário fornecer o modelo completo, siga o dele.

## Parte 2: Nota Master (MoC)

Salvar como `MoC - <Tema Geral>.md`. Entregar também o conteúdo em um único bloco de código na resposta, se o usuário pedir.

```markdown
---
tags: [moc, sintese, literatura]
data_criacao: <data de hoje, AAAA-MM-DD>
---
# 🗺️ MoC - Revisão da Literatura: <Tema Geral>

Esta nota centraliza os resumos e interconecta os padrões encontrados nos artigos analisados para facilitar a extração de dados.

## 📝 Índice de Artigos
- [[Título Curto do Artigo 1]] - Comentário de 1 linha sobre a principal contribuição (ex: "Propõe uma arquitetura baseada em X para resolver Y").
- [[Título Curto do Artigo 2]] - ...

## 🧠 Padrões e Conceitos Interconectados
- **[[Abordagens Metodológicas]]:** <Síntese de como os artigos estruturaram a pesquisa e validaram seus modelos>
- **[[Conceito Chave A]]:** <Síntese de como os artigos abordam este conceito em conjunto>
- **[[Conceito Chave B]]:** <Síntese das soluções propostas para este aspecto>

## 🚀 Lacunas Comuns e Oportunidades (Trabalhos Futuros)
<Resumo consolidado do que ainda falta ser explorado. Focar especialmente em lacunas de integração de sistemas, escalabilidade ou detecção autônoma.>
```

### Como escolher os conceitos-chave

Os exemplos do modelo (Ecossistemas Financeiros, Confiabilidade) são ilustrativos. Derive os conceitos dos próprios artigos: escolha de 2 a 5 temas que apareçam em pelo menos dois artigos e, em cada um, cite quais artigos contribuem (com `[[links]]`) e onde concordam ou divergem. O **Tema Geral** do título também sai do conjunto de artigos; se for ambíguo, escolha o mais representativo e diga qual foi na resposta.

## Regras

- **Idioma:** escreva em português, mantendo termos técnicos e nomes de métodos no original quando a tradução não for consagrada.
- **Fidelidade:** use só o que está nos artigos. Não invente números, autores, anos ou conclusões. Seção inexistente: "Não se aplica: artigo teórico". Metadado ausente: "Não identificado". Se os autores não apontam limitações, escreva "Não declaradas pelos autores" e, se fizer sentido, sinalize separadamente qualquer limitação que você mesmo perceba.
- **Paráfrase:** não copie trechos longos; citações diretas só se curtas e essenciais.
- **Síntese real:** a MoC deve comparar e conectar artigos, não repetir resumos. Atribua cada afirmação aos artigos de origem com `[[links]]`.
- **Lacunas:** baseie-se nos trabalhos futuros declarados e nas limitações apontadas; marque como "observação minha" qualquer lacuna inferida que os autores não mencionam.
- **Tamanho:** cada nota individual cabe em cerca de uma página.
- **Falhas parciais:** se um PDF estiver corrompido, protegido ou sem texto, processe os demais e liste o problema ao final.
- **Nomes duplicados:** acrescente ` (2)`, ` (3)` ao título curto e use o mesmo nome nos links.
