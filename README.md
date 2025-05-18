# SemanticORM (Python)

**SemanticORM** é uma biblioteca Python para construção programável de **modelos analíticos semânticos**, geração de **SQL dinâmico**, **documentação estruturada** e **exportação para ferramentas de BI** como Looker, Superset, Metabase e GraphQL.

> É como um ORM — mas para dados analíticos e semânticos, não apenas para CRUD de tabelas relacionais.

---

## ✨ Recursos principais

- Definição de modelos com **medidas, dimensões, filtros e fonte**
- Geração de SQL por dialetos: BigQuery, PostgreSQL, DuckDB (em breve)
- Geração de `modelo_semantico.json` compatível com múltiplas ferramentas
- Exportação para:
  - LookML (Looker)
  - Superset YAML
  - Metabase JSON
  - GraphQL SDL
  - Markdown
- Parser automático de arquivos YAML do dbt

---

## 📦 Instalação

```bash
pip install semanticorm
```

Ou, durante o desenvolvimento:

```bash
git clone https://github.com/seunome/semanticorm
cd semanticorm
pip install -e .
```

---

## 🧠 Exemplo de uso

```python
from semanticorm.model import SemanticModel

indicador = SemanticModel(
    name="indicador",
    source="fato_indicadores",
    measures=[
        {"name": "media", "type": "avg", "field": "valor"}
    ],
    dimensions=[
        {"name": "ano", "type": "temporal", "field": "data_referencia", "granularity": "year"},
        {"name": "municipio", "type": "categorical", "field": "localidade_id"}
    ],
    filters=[
        {"name": "categoria", "field": "categoria", "type": "string", "operator": "="}
    ]
)
```

---

## ⚙️ Geração de SQL

```python
sql = indicador.to_sql(dialect="bigquery", filters={"categoria": "saneamento"})
print(sql)
```

---

## 🔁 Geração e leitura de JSON

```python
indicador.to_json("modelo_semantico.json")

# E carregando depois:
from semanticorm.model import from_json
indicador = from_json("modelo_semantico.json")
```

---

## 🧩 Integração com dbt

```python
from semanticorm.tools import parse_dbt_yaml

modelo = parse_dbt_yaml("models/fato_indicadores.yml")
modelo.to_json("indicador.json")
```

---

## 📤 Exportações suportadas

### Looker (LookML)

```python
from semanticorm.export import to_looker

print(to_looker(indicador))
```

### Superset (YAML)

```python
from semanticorm.export import to_superset_yaml

print(to_superset_yaml(indicador))
```

### Metabase (JSON)

```python
from semanticorm.export import to_metabase_json

print(to_metabase_json(indicador))
```

### GraphQL (SDL)

```python
from semanticorm.export import to_graphql(indicador))
```

### Markdown

```python
from semanticorm.export import to_markdown

print(to_markdown(indicador))
```

---

## 🧱 Estrutura recomendada

```
semanticorm/
├── model.py
├── dialects/
│   └── bigquery.py
├── export/
│   ├── looker.py
│   ├── superset.py
│   ├── graphql.py
│   ├── metabase.py
│   └── markdown.py
├── tools/
│   └── parse_dbt_yaml.py
```

---

## 🛣️ Roadmap

- [x] Parser YAML do dbt
- [x] Exportação LookML, Superset, Metabase
- [x] Contrato GraphQL + Markdown
- [x] Geração de SQL dinâmica
- [ ] Execução integrada com DuckDB, SQLAlchemy (opcional)
- [ ] CLI interativo e scaffolding de modelos
- [ ] UI visual opcional para modelagem

---

## 📜 Licença

MIT

---

## 🤝 Contribuições

Sugestões e PRs são bem-vindos! Esta biblioteca visa preencher a lacuna entre engenharia de dados, produto e visualização — oferecendo uma camada semântica leve, programável e interoperável.
