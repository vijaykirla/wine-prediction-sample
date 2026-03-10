1. create a new directory with name "wine-prediction" in your local environment
2. in the directory, create a folder named "data"
3. inside the "data" folder create a dataset "wine-quality-sample" in csv format
4. create python virtual environment using command prompt "python -m venv .venv"
5. activate the environment ".venv\Scripts\activate.bat"
6. install data version control (dvc) using the command "pip install dvc"
7. intitialize dvc using "dvc init"
8. add the dataset to dvc using "dvc add data/wine-quality-sample.csv"
9. after executing the above the command metadata file with the name "wine-quality-sample.csv.dvc" will be created in "data" folder
10. store dataset in aws s3 bucket and dvc file in github
11. create s3 bucket in aws, create access key in security credentials
12. add the s3 bucket as remote using "dvc remote add -d wineremote s3://bucket-name"
13. install aws cli to configure s3 bucket
14. execute "aws configure" and enter the access key id, secret access key, default region name
15. install "pip install dvc_s3"
17. add "wine-quality-sample.csv.dvc" to git
18. git commit
19. execute "dvc push" to push dataset to s3 bucket
20. finally the .dvc folder which contains config and metadata file "wine-quality-sample.csv.dvc" in the data folder should be committed to github and the dataset to s3 bucket
    
