FROM python3

ENV FLASK_APP=hello
RUN pip install flask

ENTRYPOINT [ "flask"]
CMD [ "run", '-h', '0..0.0.0' ]