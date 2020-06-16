FROM python:3.7.2

COPY app /app

RUN pip install -r /app/requirements.txt

EXPOSE 8080

CMD ["python", "/app/main.py"]
