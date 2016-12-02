# CSV load to Elasticsearch + Kibana

See: http://www.embulk.org/docs/recipe/scheduled-csv-load-to-elasticsearch-kibana4.html#loading-a-csv-file

```
$ docker-compose up
```

```
$ embulk example ./mydata
$ embulk guess ./mydata/seed.yml -o config.yml
$ config.yml
```

```
in:
  type: file
  path_prefix: ./mydata/csv/
  decoders:
  - {type: gzip}
  parser:
    charset: UTF-8
    newline: CRLF
    type: csv
    delimiter: ','
    quote: '"'
    escape: ''
    null_string: 'NULL'
    skip_header_lines: 1
    columns:
    - {name: id, type: long}
    - {name: account, type: long}
    - {name: time, type: timestamp, format: '%Y-%m-%d %H:%M:%S'}
    - {name: purchase, type: timestamp, format: '%Y%m%d'}
    - {name: comment, type: string}
out:
  type: elasticsearch
  index: embulk
  index_type: embulk
  nodes:
  - {host: localhost}
```

```
embulk run config.yml -c diff.yml
```
