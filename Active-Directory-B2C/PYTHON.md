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
