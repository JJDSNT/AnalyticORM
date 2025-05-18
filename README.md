# AnalyticORM

Uma biblioteca leve e extensível para definição de modelos analíticos e geração dinâmica de queries estruturadas. Inspirada no Cube.js, mas com foco em simplicidade, integração flexível e uso como uma camada semântica desacoplada.

> **Objetivo:** transformar consultas analíticas complexas em uma interface declarativa, reutilizável e orientada a modelo — como um ORM, mas para análise de dados.

---

## ✨ Principais Funcionalidades

- Definição de modelos com medidas, dimensões e filtros
- Geração de consultas dinâmicas (ex: BigQuery, PostgreSQL)
- Suporte a séries temporais, agrupamentos e filtros condicionais
- Interface programável: `.sum()`, `.avg()`, `.groupBy()`, `.timeSeries()`
- Design modular para integração com pipelines, APIs ou dashboards

---

## 🚀 Instalação

(em breve no npm)

```
npm install analytic-orm
```

---

## 🧠 Conceitos

```ts
Indicador.define({
  nome: 'acesso_saneamento',
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
  .buildSQL('bigquery'); // ou 'postgresql'
```

Resultado esperado:
```sql
SELECT
  EXTRACT(YEAR FROM data_referencia) AS ano,
  localidade_id AS municipio,
  AVG(valor) AS media
FROM indicadores
WHERE categoria = 'infraestrutura'
GROUP BY ano, municipio
ORDER BY ano
```

---

## 🧩 Roadmap

- [ ] Conectores BigQuery e PostgreSQL
- [ ] Suporte a joins e modelos compostos
- [ ] Tradução de queries para JSON (modo explain/debug)
- [ ] Ferramentas de validação e inferência de esquema
- [ ] CLI opcional para scaffolding de modelos

---

## 🧱 Estrutura do Projeto

```
src/
  ├── models/
  │     └── Indicador.ts
  ├── engine/
  │     ├── builder.ts
  │     └── dialects/
  │           ├── bigquery.ts
  │           └── postgresql.ts
  ├── types/
  └── index.ts
```

---

## 📜 Licença

MIT
