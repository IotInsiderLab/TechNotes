# Active Directory B2C

Walks through how to deploy an Active Directory B2C directory and then utilize it from a Python website.

## Deploying the Azure AD B2C

1. Create The directory by follow the steps found [here](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-get-started).

  *Note: If there are multiple directories in your subscription, you may need to select the directory from the upper right menu before you can complete Step 3.*

  ![directory](img/directory.png)

2. Register your application by following the steps under **Register a web application** found [here](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-app-registration).

  In step 5, you will want to specify the Reply URL as follows

  http://localhost:5000/login/authorized

  In step 8 save the secret that you create, you won't be able to see it later.

3. Create a policy by following the steps for **Create a sign-up or sign-in policy** found [here](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-reference-policies).  

  In step 4, use the name **b2c_1_susi**

## Create a Python container for the sample

We'll create a container for Python with a copy of the code from [Azure-Samples](https://github.com/Azure-Samples/active-directory-b2c-python-flask-webapp).


1. Clone this repo locally

  ```

  git clone https://github.com/IotInsiderLab/TechNotes.git

  ```

2. From the Active-Directory-B2C folder run the following command

  ```

  docker build -t adb2cdemo .

  ```

3. To run the container interactively with the demo directory

  ```

  docker run -p 5000:5000 -ti adb2cdemo

  ```

  Goto http://localhost:5000 and you should see the sample page with a login button.

  Press Ctrl-C to exit the container

4. Now run the container with a bash shell to edit

  ```

  docker run -p 5000:5000 -ti --name myb2c adb2cdemo bash

  ```

5. Edit the sample file

  ```

  nano b2cflaskapp.py

  ```

  Update the following lines toward the top of the file.

  ```

  tenant_id = 'fabrikamb2c.onmicrosoft.com'
  client_id = 'fdb91ff5-5ce6-41f3-bdbd-8267c817015d'
  client_secret = 'X330F3#92!z614M4'

  ```

  Save and close nano

  To test your B2C directory run the following

  ```

  flask run --host=0.0.0.0

  ```

  Goto http://localhost:5000 and you should see the sample page with a login button.

  Press the login button and you will see your branded sign in sign up page.

  Login and you will see the OAuth2 data that the Python app got back from AD

  Press Ctrl-C to exit flask

  run the foolowing to exit the container

  ```

  exit

  ```

6. To save your edits in the container run the following

  ```

  docker commit myb2c

  ```

7. To run your container again

  ```

  docket start -i myb2c

  ```
