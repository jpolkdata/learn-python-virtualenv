
# learn-python-virtualenv
### Summary
Demonstrate how to use Python virtual environments.

One of the main benefits of doing this is that external packages no longer have to be installed at the system level. Now you can install them on a project-by-project basis, keeping your main Python install much cleaner. 

Note that Python versions <= 3.3 do not have venv built in. If you are using one of those lower versions then you will need to install virtualenv first

    virtualenv myenv

---
### Create the virtual environment
Inside of the project create a folder to house the virtual env files

    mkdir virtualenvs
    cd virtualenvs

Now create the virtual env

    python -m venv my_env

### Activate the virtual environment
Use the activate script to start the virtual environment

    .\my_env\Scripts\activate

You will know if it worked if you see your command prompt prefixed with (my_env)

Now if you check the Python and pip versions they will be for the packages inside of the virtual env

    python -V
    pip -V

If you check the installed packages you will just see those that are installed inside the virtual env

    python -m pip list

### Install a new package
Use pip to install a new package. It will install to the virtual environment

    pip install babel

### De-activate the virtual environment
Then deactive the virtual env and you will see the prompt revert back

    deactivate

---
### Requirements file
A requirements file can ensure that packages are synced across multiple dev team members working on the same project and virtual environment

To create this, you must first be in an active environment

    .\my_env\Scripts\activate

Now run 'pip freeze' to list the installed packages/versions

    python -m pip freeze

Now you can run that same command out to a requirements file
    python -m pip freeze > requirements.txt

How is this file used? Let's pretend we are another member of the dev team, with our own separate environment. So let's deactivate our current environment

    deactivate

and let's start up a separate environment (we will pretend this is on another machine)

    python -m venv colleague
    .\colleague\Scripts\activate

All that this colleague now needs to do in order to pull in the required packages is install directly from the requirements file

    python -m pip install -r requirements.txt

Now checking the installed packages, we see that they match the my_env virtual env

    python -m pip list