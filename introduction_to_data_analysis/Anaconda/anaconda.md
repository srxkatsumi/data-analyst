## Installing Anaconda 
Download the installer from <a href ='https://www.anaconda.com/download/'>https://www.anaconda.com/download/</a>. Choose the Python 3.7 or higher version, and the appropriate 64/32-bit installer. If you already have Python installed on your computer, this won't break anything. Instead, the default Python used by your scripts and programs will be the one that comes with Anaconda.

After installation, you’re automatically in the default conda environment with all packages installed which you can see below. You can check out your own install by entering the following command into your terminal.

    conda list
To avoid errors later, it's best to update all the packages in the default environment. Open the Terminal/ Anaconda Prompt application. In the prompt, run the following commands:

    conda upgrade conda
    conda upgrade --all
    conda install pip
    pip --version

## Using Environments
One thing that’s helped me tremendously is having separate environments for different Python versions. I used the commands below to create two separate environments - py38_env and py39_env,

    conda create -n py38_env python=3.8
    conda create -n py39_env python=3.9
    
Now, I have a general use environment for each Python version. In each of those environments, I've installed most of the standard data science packages (NumPy, SciPy, Pandas, etc.). Remember that when you set up an environment initially, you'll only start with the standard packages in addition to whatever packages you specify in your conda create statement.

I’ve also found it useful to create environments for each project I’m working on. It works great for non-data related projects too, like web apps with Flask. For example, I have an environment for my personal blog using Pelican.

## Sharing Environments
When sharing your code on GitHub, it's good practice to make an environment file and include it in the repository. You can do this using conda as:

conda env export > environment.yaml
Share the List of Dependencies
For users not using conda, you may want to share the list of packages installed in the current environment. You can use pip to generate such a list as requirements.txt file using:

    pip freeze > requirements.txt
Later, you can share this requirements.txt file with other users over Github. Once a user (or yourself) switches to another environment, you can install all the packages mentioned in the requirements.txt file using:

    pip install -r requirements.txt
