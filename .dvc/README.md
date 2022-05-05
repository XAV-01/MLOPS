# dvc config

## either 

~/.aws/credentials     
```config
[default]
    aws_access_key_id = <key>
    aws_secret_access_key = <secret>
[myprofile-xm]
    aws_access_key_id = <key>
    aws_secret_access_key = <secret>
[myprofile-cc]
    aws_access_key_id = <key>
    aws_secret_access_key = <secret>
```

.dvc/config
```config
[core]
    remote = cos-xm
['remote "remote-storage"']
    url = s3://wine-bucket-test/rain_australia/
    endpointurl = https://s3.us-south.cloud-object-storage.appdomain.cloud
['remote "cos-xm"']
    url = s3://xm-mlops/rain_australia/
    endpointurl = https://s3.eu-de.cloud-object-storage.appdomain.cloud
    region = eu-de
['remote "cos-cc"']
    url = s3://xm-mlops/rain_australia/
    endpointurl = https://s3.eu-de.cloud-object-storage.appdomain.cloud
    region = eu-de
```

## or

.dvc//config
```config
[core]
    remote = cos-xm
['remote "remote-storage"']
    url = s3://wine-bucket-test/rain_australia/
    endpointurl = https://s3.us-south.cloud-object-storage.appdomain.cloud
    access_key_id <key>
    secret_access_key <secret>
['remote "cos-cc"']
    url = s3://xm-mlops/rain_australia/
    endpointurl = https://s3.eu-de.cloud-object-storage.appdomain.cloud
    region = eu-de
    profile = myprofile-cc
    access_key_id <key>
    secret_access_key <secret>
['remote "cos-xm"']
    url = s3://xm-mlops/rain_australia/
    endpointurl = https://s3.eu-de.cloud-object-storage.appdomain.cloud
    region = eu-de
    profile = myprofile-xm
    access_key_id <key>
    secret_access_key <secret>
```
