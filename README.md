🚀 Instrumenting a Flask Application with OpenTelemetry (Self-Hosted SigNoz) 🎯

This guide provides step-by-step instructions to instrument a Flask application using OpenTelemetry for logging, tracing, and metrics, specifically for a self-hosted SigNoz setup.

---

📝 1. Configure Logging in `app.py`

Add the following lines to your `app.py` to enable logging to the console:

```python
import logging

logging.basicConfig(level=logging.DEBUG)
```

🎁 Bonus: Log Both to File & Console
To log both to the console and a file, use:

```python
logging.basicConfig(
    level=logging.DEBUG,
    format="%(asctime)s - %(levelname)s - %(message)s",
    handlers=[
        logging.FileHandler("app.log"),
        logging.StreamHandler()  # Logs to console
    ]
)
```

---

## 🛠 2. Create a Virtual Environment

Set up a virtual environment and activate it:

```sh
python -m venv venv
source ./venv/bin/activate  # On macOS/Linux
venv\Scripts\activate      # On Windows
```

---

📦 3. Install Dependencies

Install the required Python packages:

```sh
pip install opentelemetry-distro
pip install flask requests
pip install opentelemetry-exporter-otlp
```

---

⚡ 4. Run OpenTelemetry Bootstrap

Execute the following command to install necessary dependencies:

```sh
opentelemetry-bootstrap -a install
```

---

🚀 5. Run Your Application with OpenTelemetry Instrumentation for SigNoz

Replace `<APP-NAME>` with your application's name and ensure `<OTLP_ENDPOINT>` points to your self-hosted SigNoz instance (default is `http://localhost:4317` for gRPC or `http://localhost:4318` for HTTP):

```sh
OTEL_RESOURCE_ATTRIBUTES=service.name=<APP-NAME> \
OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true \
OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4317 \
OTEL_EXPORTER_OTLP_PROTOCOL=grpc \
opentelemetry-instrument --traces_exporter otlp --metrics_exporter otlp --logs_exporter otlp flask run
```

If using the HTTP endpoint, replace `grpc` with `http/protobuf`:

```sh
OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf
```

---

⚠️ 6. Prevent Auto-Reload Issues (Optional)

Auto-reload on code changes can break OpenTelemetry instrumentation. To prevent this, disable debug mode in your Flask app:

```python
if __name__ == '__main__':
    app.run(debug=False)
```

Alternatively, modify your run command to include `--no-reload`:

```sh
OTEL_RESOURCE_ATTRIBUTES=service.name=<APP-NAME> \
OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true \
OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4317 \
OTEL_EXPORTER_OTLP_PROTOCOL=grpc \
opentelemetry-instrument --traces_exporter otlp --metrics_exporter otlp --logs_exporter otlp flask run --no-reload
```

This ensures stable instrumentation while running your Flask application with SigNoz. 🎯🔥

---

Happy Coding! 🚀🐍

