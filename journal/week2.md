# Week 2 — Distributed Tracing
![cover](assets/wk2/wk2.png)

## Synopsis:
Week 2 emphasized **observability** and I learnt about its 4 pillars (metrics, logs, events and traces) as well as how crucial it is in the entire software development cycle. 

## [Required Homework](#required)
### 1. Instrument backend flask app to Open Telemetry (OTEL) using Honeycomb.io
- opened honeycomb.io
- created an honey comb account then created a new envirnment called bootcamp
- clicked API keys to get the API_HONEYCOMB_KEY
- exported as an environment variable and to saved it to gitpod's envirnment when restarted.
![honeycomb](assets/wk2/envgitpodvariables.png)
- The Service name determines the spans that get sent from the application.
- It is preferable not to set them in the work envirnment to prevent consistency. we want them set seperately so theyre specific to the services. e.g like hardcoding it in docker compose.
- so copied the OTEL_SERVICE_NAME and hard coded it into the backend section of the docker compose
- Confirguring OTEL(Open TElemetry) to send to honeycomb
Open telemetry is part of CNCF() and its a standardized method for all observeabilty tool
```
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
```
- cd backend folder
- put in the installation command from honeycomb docs to instrument a Flask app with OpenTelemetry in the requirements.txt file:
- requests are the python http client so when we make outgoing http calls, it instruments it
```
opentelemetry-api
opentelemetry-sdk
opentelemetry-exporter-otlp-proto-http
opentelemetry-instrumentation-flask
opentelemetry-instrumentation-requests
```
- run:
```pip install -r requirements.txt```
- Next step is adding the initializion lines to the flaskapp.py file and make comments to distinguish. These updates will create and initialize a tracer and Flask instrumentation to send data to Honeycomb:
```python
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
```
- Next
```python
# HoneyComb -----------
# Initialize tracing and an exporter that can send data to Honeycomb
provider = TracerProvider()
processor = BatchSpanProcessor(OTLPSpanExporter())
provider.add_span_processor(processor)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer(__name__)
```
- Next
```python
# Initialize automatic instrumentation with Flask
app = Flask(__name__)
FlaskInstrumentor().instrument_app(app)
RequestsInstrumentor().instrument()
```
- Testing the setup
    - Already had my frontend initialised thanks to the gitpod configuration
    - cd to root folder
    - Docker Compose up
    - All set and receiving data
![honeycomb](assets/wk2/)
![honeycomb](assets/wk2/)
### 2. Run Queries to explore traces within Honeycomb.io
### 3. Instrument AWS X-Ray into backend flask app
### 4. Configure, provision X-Ray daemon within docker-compose 
### 5. Observe X-Ray in AWS console
### 6. Integrate Rollbar and capture error
### 7. Configure custom logger to send CloudWatch Logs

## [Homework Challenges](#challenges)

## References:
- [Open telemetry for python-Honeycomb.io Documentation](https://docs.honeycomb.io/getting-data-in/opentelemetry/python/)
- [honeycomb-whoami.glitch.me](honeycomb-whoami.glitch.me)
