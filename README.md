# Smart Price Monitor Agent

Proyecto academico y de portafolio para la **Construccion de un Agente de Software con Ciclo de Vida DevSecOps**. El sistema implementa un agente autonomo de monitoreo de precios con FastAPI, LangChain, OpenAI, SQLite, pruebas automatizadas y pipeline DevSecOps completo.

![Architecture Diagram](docs/architecture.png)

## Caracteristicas Principales

- API REST en FastAPI con interfaz web ligera para demo.
- Agente autonomo que recibe un producto y un precio objetivo.
- Consulta precio desde API publica o modo simulacion deterministica.
- Genera recomendacion inteligente usando LangChain y OpenAI cuando existe `OPENAI_API_KEY`.
- Persiste historial de ejecuciones en SQLite.
- Sanitizacion de entradas y manejo robusto de errores.
- Pipeline CI con lint, tests, cobertura, `pip-audit` y build Docker.
- Despliegue reproducible en Render mediante `render.yaml`.

## Arquitectura

- **Cliente**: navegador o consumidor REST.
- **API**: FastAPI expone endpoints para monitoreo, historial y salud.
- **Agente**: orquesta consulta, decision, analisis y persistencia.
- **Servicios**: precio, analisis LLM e historial.
- **Persistencia**: SQLite para historico y trazabilidad.
- **Servicios externos**: DummyJSON y OpenAI API.

Documentacion adicional:

- [C4 Model](docs/c4_diagram.md)
- [Requisitos de calidad](docs/requisitos_calidad.md)
- [Presentacion](docs/presentacion.md)

## Estructura del Proyecto

```text
.
|-- docs/
|   |-- architecture.png
|   |-- c4_diagram.md
|   |-- presentacion.md
|   `-- requisitos_calidad.md
|-- src/
|   |-- main.py
|   |-- agent/
|   |-- database/
|   |-- security/
|   |-- services/
|   `-- utils/
|-- tests/
|   |-- test_agent.py
|   `-- test_services.py
|-- .github/workflows/ci.yml
|-- .dockerignore
|-- .env.example
|-- Dockerfile
|-- README.md
|-- render.yaml
`-- requirements.txt
```

## Stack Tecnologico

- Python 3.12
- FastAPI
- LangChain + OpenAI API
- SQLite
- Pytest + pytest-cov
- Black + Flake8
- pip-audit
- Docker
- GitHub Actions
- Render

## Requisitos Previos

- Python 3.12
- `pip`
- Docker Desktop
- Cuenta en Render o Railway
- Clave `OPENAI_API_KEY` si deseas analisis con modelo real

## Instalacion Local

```bash
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
.venv\Scripts\activate     # Windows PowerShell
pip install -r requirements.txt
cp .env.example .env
```

Configura al menos estas variables:

- `DATABASE_URL=sqlite:///./price_monitor.db`
- `PRICE_PROVIDER_MODE=simulation`
- `OPENAI_API_KEY=<tu_clave>`

## Ejecucion

```bash
uvicorn src.main:app --reload --host 0.0.0.0 --port 8000
```

Aplicacion:

- Landing page: [http://localhost:8000](http://localhost:8000)
- Documentacion OpenAPI: [http://localhost:8000/docs](http://localhost:8000/docs)
- Healthcheck: [http://localhost:8000/health](http://localhost:8000/health)

### Ejemplo de solicitud

```bash
curl -X POST http://localhost:8000/api/v1/monitor \
  -H "Content-Type: application/json" \
  -d '{"product_name":"Mechanical Keyboard","target_price":149.99}'
```

## Testing y Calidad

### Ejecutar pruebas

```bash
pytest --cov=src --cov-report=term-missing --cov-fail-under=70
```

### Formato

```bash
black src tests
black --check src tests
```

### Lint

```bash
flake8 src tests --max-line-length=100 --exclude=__pycache__
```

### Auditoria de dependencias

```bash
pip-audit -r requirements.txt
```

## Pipeline DevSecOps

El workflow `/.github/workflows/ci.yml` ejecuta automaticamente:

1. Instalacion de dependencias
2. Verificacion de formato con Black
3. Lint con Flake8
4. Tests con cobertura minima del 70%
5. Auditoria de dependencias con `pip-audit`
6. Build Docker
7. Validacion final con compilacion del codigo

## Docker

### Build

```bash
docker build -t smart-price-monitor-agent .
```

### Run

```bash
docker run --rm -p 8000:8000 --env-file .env smart-price-monitor-agent
```

## Despliegue en Render

1. Crea un repositorio en GitHub y sube este proyecto.
2. En Render selecciona **New +** -> **Blueprint**.
3. Conecta el repositorio y Render detectara `render.yaml`.
4. Define `OPENAI_API_KEY` en el panel de variables de entorno.
5. Despliega y valida `https://<tu-servicio>.onrender.com/health`.

## Capturas y Demo

- `docs/architecture.png` resume la arquitectura propuesta.
- La landing page incluida permite una demo visual sin frontend adicional.
- La respuesta del agente se presenta en JSON para facilitar pruebas academicas y tecnicas.

## Decisiones Tecnicas

- **FastAPI** por productividad, validacion y documentacion integrada.
- **SQLite** por simplicidad y foco academico.
- **LangChain** para encapsular prompts y aislar el proveedor de LLM.
- **Modo simulacion** para demos reproducibles y resiliencia sin red.
- **Capas modulares** para respetar SOLID y simplificar pruebas.

## Practicas DevSecOps Implementadas

- Secretos externalizados en variables de entorno.
- Sanitizacion estricta de entrada.
- Auditoria de dependencias.
- Validaciones automaticas en CI.
- Imagen Docker reproducible.
- Despliegue declarativo con `render.yaml`.

## Estado del Proyecto

Listo para demostracion academica y como base de portafolio profesional. Solo requiere configurar `OPENAI_API_KEY` y publicar el repositorio para activar el despliegue continuo.
