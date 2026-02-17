# Usa uma imagem oficial do Python como base
FROM python:3.12-slim

# Define o diretório de trabalho
WORKDIR /app

# Copia o arquivo requirements.txt e instala as dependências
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip setuptools wheel \
    && pip install --no-cache-dir -r requirements.txt

# Copia o restante da aplicação para o diretório de trabalho
COPY . /app

# Cria a pasta para armazenar as métricas do Prometheus
RUN mkdir -p /tmp/prometheus_metrics

# Configura a variável de ambiente para Prometheus
ENV PROMETHEUS_MULTIPROC_DIR=/tmp/prometheus_metrics

# Configura a variável de ambiente do Flask
ENV FLASK_APP=index.py

RUN chmod +x /app/entrypoint.sh

# Expõe a porta que o Flask irá usar
EXPOSE 5000

# Define o entrypoint para executar o script de entrada
ENTRYPOINT ["./entrypoint.sh"]
