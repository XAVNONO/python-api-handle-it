FROM python:3.7

COPY .pylintrc /

RUN pip3 install pylint 
RUN pip3 install pylint-json2html

CMD ["/bin/bash", "-c", "pylint /app/ > /app/reports/pylint/report.json; pylint-json2html -o /app/reports/pylint/report.html /app/reports/pylint/report.json"]
