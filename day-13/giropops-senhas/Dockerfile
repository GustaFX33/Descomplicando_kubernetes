# Estágio 1: Build (Instalação de dependências)
FROM cgr.dev/chainguard/python:latest-dev AS builder

WORKDIR /app

# Criar venv e instalar dependências primeiro para aproveitar o cache
RUN python -m venv venv
ENV PATH="/app/venv/bin:$PATH"

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Estágio 2: Produção (Imagem final leve e segura)
FROM cgr.dev/chainguard/python:latest

WORKDIR /app

# Copia apenas o ambiente virtual do estágio de build
COPY --from=builder /app/venv /app/venv

# Copia o código da aplicação e pastas necessárias
COPY app.py .
COPY templates/ templates/
COPY static/ static/

# Configura o PATH para usar o venv
ENV PATH="/app/venv/bin:$PATH"

# Flask escuta na 5000 por padrão
EXPOSE 5000

# Se estiver usando Flask diretamente (melhor usar Gunicorn em prod)
ENTRYPOINT ["python", "app.py"]
