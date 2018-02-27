# copy from S3

Copy from s3 to stdout :
```bash
aws s3 cp "s3://$bukcet/$key" -
```

Display progression (bytes, rate, timer, Named) and add a buffer :
```bash
pv -cbrtN aws -B 512m
```

Copy from Stdin : 
```bash
psql -c "COPY table FROM STDIN WITH (FORMAT CSV, DELIMITER ',', QUOTE '\"')"
```

In case of trouble with Mull value as `""` : `sed s/,\"\"/,/g`


The final command : 
```bash
aws s3 cp "s3://$bukcet/$key" - \
  | pv -cbrtN aws -B 100m \
  | sed s/,\"\"/,/g \
  | psql -c "COPY table FROM STDIN WITH (FORMAT CSV, DELIMITER ',', QUOTE '\"')"
```
