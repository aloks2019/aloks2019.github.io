# ora-py-lambda
How to use Lambda (Python) to connect to an Oracle DB with with all required dependicies.

# Introduction 
This repositories shows how to build a Lambda (Python) function with all required dependicies that can connect to RDS Oracle Instance.
I used linux environment to build all dependent packages.

# Step1 : Oracle Clinet Requirements
Download and unzip oracle instant basic clinet from https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html. I used instantclient-basic-linux.x64-12.2.0.1.0.zip.

$:/home/Lambda/ora12_client > ls -ltr 
-rw-r--r-- xxxx xxxx xxxx instantclient-basic-linux.x64-12.2.0.1.0.zip

$:/home/Lambda/ora12_client > unzip instantclient-basic-linux.x64-12.2.0.1.0.zip

$:/home/Lambda/ora12_client > ls -ltr 
-rw-r--r-- xxxx xxxx xxxx instantclient-basic-linux.x64-12.2.0.1.0.zip
-rw-r--r-- xxxx xxxx xxxx instantclient_12_2


# Step 2 :Ceate virtual environment and install cx_Oracle
$:/home/Lambda > mkdir ora_py_lambda

$:/home/Lambda > virtualenv ora_py_lambda

$:/home/Lambda > source ora_py_lambda/bin/activate

(ora_py_lambda) $:/home/Lambda > pip install cx_Oracle

(ora_py_lambda) $:/home/Lambda > deactivate

# Step 3: Create dependicies for oracle client
$:/home/Lambda > cp /home/Lambda/ora12_client/instantclient_12_2/*  /home/Lambda/ora_py_lambda/lib

$:/home/Lambda > cd ora_py_lambda

$:/home/Lambda/ora_py_lambda > cp /lib64/libaio.so.1.0.1 lib

$:/home/Lambda/ora_py_lambda > ln -s /var/task/libaio.so.1.0.1 /home/Lambda/ora_py_lambda/lib/libaio.so.1

$:/home/Lambda/ora_py_lambda > ln -s /var/task/libaio.so.1.0.1 /home/Lambda/ora_py_lambda/lib/libaio.so

$:/home/Lambda/ora_py_lambda > ln -s /var/task/lib/libaio.so.1.0.1 /home/Lambda/ora_py_lambda/libaio.so.1.0.1

$:/home/Lambda/ora_py_lambda > ln -s /var/task/lib/libclntsh.so.12.1 /home/Lambda/ora_py_lambda/lib/libclntsh.so

# Step 4: Create deployment package
$:/home/Lambda/ora_py_lambda > cd lib/python2.7/site-packages/

$:/home/Lambda/ora_py_lambda/lib/python2.7/site-packages > zip -r9 /home/Lambda/ora_py_lambda.zip .

$:/home/Lambda/ora_py_lambda/lib/python2.7/site-packages > cd /home/Lambda/ora_py_lambda/

$:/home/Lambda/ora_py_lambda >  zip --symlinks -r9 /home/Lambda/ora_py_lambda.zip lib/*

$:/home/Lambda/ora_py_lambda > vi lambda_functcion.py <<put yout python code here in this file>>

$:/home/Lambda/ora_py_lambda > zip -g /home/Lambda/ora_py_lambda.zip lambda_function.py

# Step 5: Update Lambda with your deployment package
Now you need to upload ora_py_lambda.zip in S3 bucket as direct upload to lambda will not work because of it's size. After uploading to S3, you need to update/create lambda using this S3 location.
