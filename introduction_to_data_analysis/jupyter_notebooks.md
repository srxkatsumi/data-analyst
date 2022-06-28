## What are Jupyter Notebooks?
Welcome to this lesson on using Jupyter notebooks. The notebook is a web application that allows you to combine explanatory text, math equations, code, and visualizations all in one easily sharable document. For example, here's one of my favorite notebooks shared recently, binary black hole signals in LIGO open data detected by the LIGO experiment. You could download the data, run the code in the notebook, and repeat the analysis, in effect detecting the gravitational waves yourself! You can view a few more tutorial notebooks at Gravitational Wave Open Science Center homepage.

## Literate Programming
Notebooks are a form of literate programming proposed by Donald Knuth in 1984. With literate programming, the documentation is written as a narrative alongside the code instead of sitting off by its own. In Donald Knuth's words,
     _Instead of imagining that our main task is to instruct a computer what to do, let us concentrate rather on explaining to human beings what we want a computer to do._
      
After all, code is written for humans, not for computers. Notebooks provide exactly this capability. You are able to write documentation as narrative text, along with code. This is not only useful for the people reading your notebooks, but for your future self coming back to the analysis.

Just a small aside: recently, this idea of literate programming has been extended to a whole programming language, Eve.


## Instalando o Notebook Jupyter     

The Jupyter notebooks automatically get installed with the Anaconda distribution. You'll be able to use notebooks from the default environment.
To run the notebook, run the following command at the Terminal (Mac/Linux) or Command Prompt (Windows) / Anaconda Prompt (Windows):

    jupyter notebook

## Launching the Notebook Server
To start a notebook server using a command-line interface, open the Terminal/Anaconda prompt, and navigate to the directory where you'd like to create notebook files (.ipynb). You can confirm the present working directory using pwd.

    cd <directory_path>
    pwd

Next, enter the following command in your terminal/Anaconda prompt

    jupyter notebook

Shutting down Jupyter Notebook

You can shutdown the entire server by pressing control + C twice in the terminal. Again, this will immediately shutdown all the running notebooks, so make sure your work is saved!


## Markdown Cells
As with code cells, you press Shift + Enter or Control + Enter to run the Markdown cell, where it will render the Markdown to formatted text. 

### Headers 
     # Header 1
     ## Header 2
     ### Header 3
### Links
Linking in Markdown is done by enclosing text in square brackets and the URL in parentheses, like this
     [Linkedin Victoria](https://www.linkedin.com/in/victoriagcosta/)
     
### Emphasis
You can add emphasis through bold or italics with asterisks or underscores (* or _). For italics, wrap the text in one asterisk or underscore, _gelato_ or *gelato* renders as gelato.

Bold text uses two symbols, **aardvark** or __aardvark__ looks like aardvark.

Either asterisks or underscores are fine as long as you use the same symbol on both sides of the text.

### Creating a Slideshow
#### Running the slideshow
To create the slideshow from the notebook file, you'll need to use nbconvert:

    jupyter nbconvert notebook.ipynb --to slides
This just converts the notebook to the necessary files for the slideshow, but you need to serve it with an HTTP server to actually see the presentation.

To convert it and immediately see it, use

     jupyter nbconvert notebook.ipynb --to slides --post serve
