## train
python3 model_train_pipeline.py ../../Test_Project/my-model_v1/src/breast_cancer.csv ../../Test_Project/my-model_v1/ 10 ./pickle_model

## deploy
python3 model_deploy_pipeline.py ./pickle_model ../../Test_Project/my-model_v1/ ../../credentials.yaml
python3 model_deploy_pipeline.py ../../../models/model.joblib ../../../ ../../../credentials.yaml

## validate
python3 model_deployed_validate_pipeline.py ../../Test_Project/my-model_v1/src/breast_cancer.csv  ../../credentials.yaml ../../Test_Project/my-model_v1/

## update
python3 model_update_pipeline.py ./pickle_model ../../credentials.yaml ../../Test_Project/my-model_v1/

## redeploy
python3 model_redeploy_pipeline.py ../../credentials.yaml
