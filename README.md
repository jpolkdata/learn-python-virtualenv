
# learn-python-virtualenv
### Summary
Demonstrate how to use Python virtual environments. The learnings here are from the Pluralsight course **Managing Python Packages and Virtual Environments** by **Reindert-Jan Ekker**. This example was run on a Windows machine using Visual Studio Code as the editor.

One of the main benefits of using Python virtual environments is that external packages no longer have to be installed at the system level. By installing the external packages inside of a virtual environment you can install them on a project-by-project basis, keeping your main Python install much less cluttered. 

Note that Python versions <= 3.3 do not have venv built in. If you are using one of those lower versions then you will need to install virtualenv first.

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

Now if you check the Python and pip versions they will be specifically for the packages inside of the virtual env

    python -V
    pip -V

If you check the installed packages you will just see those that are installed inside the virtual env

    python -m pip list

### Install a new package
Use pip to install a new package. It will install to the virtual environment. We are using the Babel package
for this example, which provides functionality for localization and internationalization. The main site for 
this package is located at https://babel.pocoo.org/en/latest/.

    pip install babel

Because we have activated the virtual env, this will install just to that environment and NOT to the main
Python installation. You can re-check the installed packages to confirm.

    python -m pip list

### De-activate the virtual environment
Now deactive the virtual env and you will see the command prompt revert back to the default. If you again
check the list of installed packages, you will not see Babel in the list.

    deactivate
    python -m pip list

---
### Requirements file
A requirements file can ensure that packages are synced across multiple dev team members working on the same project and virtual environment

To create this, you must first be in an active environment

    .\my_env\Scripts\activate

Now run 'pip freeze' to list the installed packages/versions. The difference between the 'freeze' command and the list command that
we have used to this point is that the freeze command will list the packages along with their current version. This is important
because you want anyone that would import these packages for this same project to be using the same versions of those tools as
what you are currently using.

    python -m pip freeze

Now you can run that same command out to a requirements file
    python -m pip freeze > requirements.txt

**NOTE**: The output will contain == to specify that the version must match what is described in the requirements file. However, you can actually change how the versions are specified here. 

Examples:

    Babel==2.11.0     # Must be version 2.11.0
    Babel>=2.11.0     # Minimum version 2.11.0
    Babel!=2.11.0     # Anything except version 2.11.0
    'Babel<2.11.0'    # Anything earlier than version 2.11.0

Note that when you use < or > you must wrap it in single quotes, as these characters have special meaning on the command line

How is this file used? Let's pretend we are another member of the dev team, with our own separate environment. So let's deactivate our current environment and start up a separate environment (we will pretend this is on another machine)

    deactivate
    python -m venv colleague
    .\colleague\Scripts\activate

All that this colleague now needs to do in order to pull in the required packages is install directly from the requirements file

    python -m pip install -r requirements.txt

Now checking the installed packages, we see that they match the my_env virtual env

    python -m pip list

Now deactivate the environment

    deactivate

---
### Next Steps
In this example we have placed our virtual env under subfolders within the project. You may find that many people will actually create the virtual env directly under the project folder itself. Also, many people will name it as 'venv' like so

    python -m venv venv

You can use a package called 'tox' to check that your project runs against multiple versions of Python. This will basically create a virtual env for each Python version it is configured to run against, and that you have installed on your machine. It can create the env, run the unit tests, then repeat for each version and give you output from the unit tests.