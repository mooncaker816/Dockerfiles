FROM elasticsearch:6.4.1
RUN mkdir -p /usr/share/elasticsearch/plugins/ik
RUN mkdir -p /usr/share/elasticsearch/plugins/pinyin
COPY elasticsearch-analysis-ik-6.4.1 /usr/share/elasticsearch/plugins/ik
COPY elasticsearch-analysis-pinyin-6.4.1 /usr/share/elasticsearch/plugins/pinyin