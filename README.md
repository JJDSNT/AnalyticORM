# SemanticORM (JS/TS)

**SemanticORM** é uma biblioteca leve e extensível para definição programável de **modelos analíticos** e geração de **queries dinâmicas estruturadas**. Inspirada no Cube.js, mas com foco em simplicidade, flexibilidade de integração e uso como uma **camada semântica desacoplada e interoperável**.

> É como um ORM — mas para análise de dados: define medidas, dimensões e filtros, gera SQL sob demanda, e exporta para múltiplas ferramentas de BI e visualização.

---

## ✨ Principais Funcionalidades

- Definição de modelos com **medidas, dimensões, filtros e fonte**
- Geração de SQL por dialetos (`bigquery`, `postgresql`, `duckdb`)
- Exportação de modelos para:
  - Looker (LookML)
  - Superset (YAML)
  - Metabase (JSON)
  - GraphQL (SDL)
  - Markdown
- Importação automática de modelos a partir de arquivos YAML do dbt
- Suporte ao formato JSON semântico compatível com múltiplas ferramentas
- Integração com APIs, SSR, dashboards, pipelines ou visualizações

---

## 🚀 Instalação

```bash
npm install semantic-orm
```

---

## 🧠 Conceito

```ts
Indicador.define({
  nome: 'acesso_saneamento',
  fonte: 'fato_indicadores',
  medidas: {
    media: { tipo: 'avg', campo: 'valor' },
    total: { tipo: 'sum', campo: 'valor' }
  },
  dimensoes: {
    ano: { campo: 'data_referencia', tipo: 'ano' },
    municipio: { campo: 'localidade_id' }
  },
  filtros: {
    categoria: { campo: 'categoria', operador: '=' }
  }
});
```

---

## 📊 Exemplo de Consulta

```ts
const query = Indicador
  .select('media')
  .groupBy('ano', 'municipio')
  .filter({ categoria: 'infraestrutura' })
  .timeSeries('ano')
  .buildSQL('bigquery');
```

Resultado:

```sql
SELECT
  EXTRACT(YEAR FROM data_referencia) AS ano,
  localidade_id AS municipio,
  AVG(valor) AS media
FROM fato_indicadores
WHERE categoria = 'infraestrutura'
GROUP BY ano, municipio
ORDER BY ano
```

---

## 🧩 Exportações suportadas

```ts
import {
  exportLookML,
  exportSupersetYAML,
  exportMetabaseJSON,
  exportGraphQLSchema,
  exportMarkdownDoc
} from 'semantic-orm/export';

const model = Indicador.getModel();

fs.writeFileSync('indicador.view.lkml', exportLookML(model));
fs.writeFileSync('indicador_superset.yaml', exportSupersetYAML(model));
fs.writeFileSync('indicador_metabase.json', exportMetabaseJSON(model));
fs.writeFileSync('indicador.graphql', exportGraphQLSchema(model));
fs.writeFileSync('indicador.md', exportMarkdownDoc(model));
```

---

## 🔁 Interoperabilidade via JSON

```ts
Indicador.loadFromJson('modelo_semantico.json');
Indicador.exportToJson('modelo_semantico.json');
```

Esse JSON pode ser compartilhado com a versão Python do `semanticorm` ou usado em pipelines externas.

---

## 🧱 Estrutura do Projeto

```
src/
  ├── models/
  │     └── Indicador.ts
  ├── engine/
  │     └── builder.ts
  ├── dialects/
  │     ├── bigquery.ts
  │     ├── postgresql.ts
  │     └── duckdb.ts
  ├── export/
  │     ├── looker.ts
  │     ├── superset.ts
  │     ├── metabase.ts
  │     ├── graphql.ts
  │     └── markdown.ts
  ├── tools/
  │     └── parseDbtYaml.ts
  ├── types/
  └── index.ts
```

---

## 🛣️ Roadmap

- [x] Geração de SQL sob demanda
- [x] Exportação LookML, Superset, Metabase
- [x] Exportação GraphQL + Markdown
- [x] JSON interoperável entre JS/TS e Python
- [ ] CLI para scaffolding de modelos e preview
- [ ] UI visual para criação de modelos analíticos

---

## 📜 Licença

MIT

---

## 🤝 Contribuições

PRs, sugestões e feedbacks são bem-vindos! O objetivo é construir uma camada semântica moderna, programável e aberta — conectando dados a decisões com simplicidade e poder.
