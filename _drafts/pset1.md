---
layout: post
title: "Problem Set 1"
---


1. Let $$x_{1},…,x_{N}$$ be $$N$$ boolean variables, taking on the values 0 or 1. 
   - Consider the conjunction $$\bar{x}_{1}\wedge\bar{x}_{2}\cdots\wedge\bar{x}_{n}\wedge x_{n+1}\wedge\cdots\wedge x_{N}$$ in which the first $$n$$ variables are negated. Express this function as an LT neuron $$H\left(\sum_{i}w_{i}x_{i}-\theta\right)$$ with weights $$w_{i}$$ and threshold $$\theta$$, where $$H$$ is the Heaviside step function. Note that conjunction, or AND, is defined by $$x_{1}\wedge\cdots\wedge x_{N}=1\text{ if and only if }x_{i}=1\text{ for all }i$$

   - Do the same for the disjunction $$\bar{x}_{1}\vee\bar{x}_{2}\cdots\vee\bar{x}_{n}\vee x_{n+1}\vee\cdots\vee x_{N}$$. Note that disjunction, or OR, is defined by $$x_{1}\vee\cdots\vee x_{N}=1\text{ if and only if }x_{i}=1\text{ for some }i$$

2. Introduction to the MNIST dataset. This dataset was created at AT&T Bell Labs in the 1990s by modifying a larger dataset from NIST. It consists of images of handwritten digits, and was heavily used in machine learning research in the 1990s and 2000s.  [Documentation](http://yann.lecun.com/exdb/mnist/) about the dataset can be found at Yann LeCun's personal website.
<br><br>
If you are using your own computer, download the sample code using the command
```
$ git clone https://github.com/cos495/code.git
$ cd code
$ julia
julia> Pkg.add("IJulia")
julia> using IJulia
julia> notebook()
```
**IJulia** is the Julia flavor of the Jupyter notebook. If it doesn't install properly, you may have to consult the IJulia [documentation](https://github.com/JuliaLang/IJulia.jl).
<br><br>
If you are using JuliaBox, point your browser to juliabox.com.  Select "Sync" in the toolbar, type `https://github.com/cos495/code.git` into the box under "Git Clone URL", and press the "+" button on the right. Now select "Jupyter" in the toolbar.
<br><br>
Either of these methods should open the Jupyter Notebook Dashboard in your browser. Select the "Files" tab if necessary, select the `code` directory, and then select `MNIST-Intro.ipynb`. The notebook should open in your browser.
   - Executing the code in the notebook cells will enable you to visualize the MNIST images.
   
   - Write code to compute the pixelwise standard deviation of the images in each digit class.
   
   - For each digit class, visualize the standard deviation as an image.


3. Classifying MNIST images with an LT neuron.  Return to the Notebook Dashboard and select `DeltaRule.ipynb`.
   - The sample code trains an LT neuron to be activated by images of “two” while remaining inactive for images of other digits. Run the code. What is the final average training error of the LT neuron? 

   - Write code to run the trained LT neuron on the test set of images. What is the average error on the test set? Submit your code.

   - What would be the error rate of the trivial recognition algorithm that always returns an output of 0?
