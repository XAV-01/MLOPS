# 🧬 DVC CI/CD MLOps Pipeline
MLOps pipeline with DVC and CML using Github Actions and IBM Cloud


[![model-deploy-on-release](https://github.com/XAV-01/MLOPS-weather/actions/workflows/train_evaluate.yaml/badge.svg)](https://github.com/XAV-01/MLOPS-weather/actions/workflows/train_evaluate.yaml)
[![Python Package and Test](https://github.com/XAV-01/MLOPS-weather/actions/workflows/test_on_push.yaml/badge.svg)](https://github.com/XAV-01/MLOPS-weather/actions/workflows/test_on_push.yaml)

[Video Demo](https://www.youtube.com/watch?v=URpGaE-FA5U)

[Documentation and Implementation Guide](https://mlops-guide.github.io)

## 🔰 Milestones
- [X] Data Versioning: DVC
- [X] Machine Learning Pipeline: DVC Pipeline (preprocess, train, evaluate)
- [X] CI/CD: Unit testing with Pytest, pre-commit and Github Actions
- [X] CML: Continuous Machine Learning and Github Actions
- [X] Deploy on release: Github Actions and IBM Watson
- [ ] Monitoring: OpenScale
- [ ] Infrastructure-as-a-code: Terraform script

## 📋 Requirements

* DVC
* Python3 and pip
* Access to IBM Cloud Object Storage

## 🏃🏻 Running Project

### 🔑 Setup IBM Bucket Credentials

#### MacOS
Setup your credentials on ```~/.aws/credentials``` and ```~/.aws/config```. DVC works perfectly with IBM Obejct Storage, although it uses S3 protocol, you can also see this in other portions of the repository.


~/.aws/credentials

```credentials
[default]
aws_access_key_id = {{Key ID}}
aws_secret_access_key = {{Access Key}}
```

#### Githut secret

Go to  settings in your project

### ✅ Pre-commit Testings

In order to activate pre-commit testing you need ```pre-commit```

Installing pre-commit with pip
```
pip install pre-commit
```

Installing pre-commit on your local repository. Keep in mind this creates a Github Hook.
```
pre-commit install
```

Now everytime you make a commit, it will run some tests defined on ```.pre-commit-config.yaml``` before allowing your commit.

**Example**
```
$ git commit -m "Example commit"

black....................................................................Passed
pytest-check.............................................................Passed
```


### ⚗️ Using DVC

Download data from the DVC repository(analog to ```git pull```)
```
dvc pull
```

Reproduces the pipeline using DVC
```
dvc repro
```


### ⚙️ DVC Pipelines


✂️ Preprocessing pipeline
```
dvc run -n preprocess -d ./src/preprocess_data.py -d data/weatherAUS.csv \
-o ./data/weatherAUS_processed.csv -o ./data/features.csv \
python3 ./src/preprocess_data.py ./data/weatherAUS.csv
```


📘 Training pipeline
```
dvc run -n train -d ./src/train.py -d ./data/weatherAUS_processed.csv \
 -d ./src/model.py \
-o ./models/model.joblib \
python3 ./src/train.py ./data/weatherAUS_processed.csv ./src/model.py 200
```


📊 Evaluate pipeline
```
dvc run -n evaluate -d ./src/evaluate.py -d ./data/weatherAUS_processed.csv \
-d ./src/model.py -d ./models/model.joblib -o ./results/metrics.json \
-o ./results/precision_recall_curve.png -o ./results/roc_curve.png \
python3 ./src/evaluate.py ./data/weatherAUS_processed.csv ./src/model.py ./models/model.joblib
```

### 🐙 Git Actions
🔐 IBM Credentials


Fill the ```credentials_example.yaml``` file and rename it to ```credentials.yaml``` to be able to run the scripts that require IBM keys. ⚠️ Never upload this file to GitHub!

To use Git Actions to deploy your model, you'll need to encrypt it, to do that run the command bellow and choose a strong password.

```
gpg --symmetric --cipher-algo AES256 credentials.yaml 
```
Now in the GitHub page for the repository, go to ```Settings->Secrets``` and add the keys to the following secrets:

```
AWS_ACCESS_KEY_ID (Bucket Credential)
AWS_SECRET_ACCESS_KEY (Bucket Credential)
IBM_CREDENTIALS_PASS (password for the encrypted file)
```


![svg image](data:image/svg+xml,<svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-gear UnderlineNav-octicon d-none d-sm-inline">
    <path fill-rule="evenodd" d="M7.429 1.525a6.593 6.593 0 011.142 0c.036.003.108.036.137.146l.289 1.105c.147.56.55.967.997 1.189.174.086.341.183.501.29.417.278.97.423 1.53.27l1.102-.303c.11-.03.175.016.195.046.219.31.41.641.573.989.014.031.022.11-.059.19l-.815.806c-.411.406-.562.957-.53 1.456a4.588 4.588 0 010 .582c-.032.499.119 1.05.53 1.456l.815.806c.08.08.073.159.059.19a6.494 6.494 0 01-.573.99c-.02.029-.086.074-.195.045l-1.103-.303c-.559-.153-1.112-.008-1.529.27-.16.107-.327.204-.5.29-.449.222-.851.628-.998 1.189l-.289 1.105c-.029.11-.101.143-.137.146a6.613 6.613 0 01-1.142 0c-.036-.003-.108-.037-.137-.146l-.289-1.105c-.147-.56-.55-.967-.997-1.189a4.502 4.502 0 01-.501-.29c-.417-.278-.97-.423-1.53-.27l-1.102.303c-.11.03-.175-.016-.195-.046a6.492 6.492 0 01-.573-.989c-.014-.031-.022-.11.059-.19l.815-.806c.411-.406.562-.957.53-1.456a4.587 4.587 0 010-.582c.032-.499-.119-1.05-.53-1.456l-.815-.806c-.08-.08-.073-.159-.059-.19a6.44 6.44 0 01.573-.99c.02-.029.086-.075.195-.045l1.103.303c.559.153 1.112.008 1.529-.27.16-.107.327-.204.5-.29.449-.222.851-.628.998-1.189l.289-1.105c.029-.11.101-.143.137-.146zM8 0c-.236 0-.47.01-.701.03-.743.065-1.29.615-1.458 1.261l-.29 1.106c-.017.066-.078.158-.211.224a5.994 5.994 0 00-.668.386c-.123.082-.233.09-.3.071L3.27 2.776c-.644-.177-1.392.02-1.82.63a7.977 7.977 0 00-.704 1.217c-.315.675-.111 1.422.363 1.891l.815.806c.05.048.098.147.088.294a6.084 6.084 0 000 .772c.01.147-.038.246-.088.294l-.815.806c-.474.469-.678 1.216-.363 1.891.2.428.436.835.704 1.218.428.609 1.176.806 1.82.63l1.103-.303c.066-.019.176-.011.299.071.213.143.436.272.668.386.133.066.194.158.212.224l.289 1.106c.169.646.715 1.196 1.458 1.26a8.094 8.094 0 001.402 0c.743-.064 1.29-.614 1.458-1.26l.29-1.106c.017-.066.078-.158.211-.224a5.98 5.98 0 00.668-.386c.123-.082.233-.09.3-.071l1.102.302c.644.177 1.392-.02 1.82-.63.268-.382.505-.789.704-1.217.315-.675.111-1.422-.364-1.891l-.814-.806c-.05-.048-.098-.147-.088-.294a6.1 6.1 0 000-.772c-.01-.147.039-.246.088-.294l.814-.806c.475-.469.679-1.216.364-1.891a7.992 7.992 0 00-.704-1.218c-.428-.609-1.176-.806-1.82-.63l-1.103.303c-.066.019-.176.011-.299-.071a5.991 5.991 0 00-.668-.386c-.133-.066-.194-.158-.212-.224L10.16 1.29C9.99.645 9.444.095 8.701.031A8.094 8.094 0 008 0zm1.5 8a1.5 1.5 0 11-3 0 1.5 1.5 0 013 0zM11 8a3 3 0 11-6 0 3 3 0 016 0z"></path>
</svg>)