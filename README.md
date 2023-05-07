Download Link: https://assignmentchef.com/product/solved-ml-exercise1-linear-regression
<br>
In this exercise, you will implement linear regression and get to see it workon data. Before starting on this programming exercise, we strongly recom&#x2;mend watching the video lectures and completing the review questions forthe associated topics.To get started with the exercise, you will need to download the startercode and unzip its contents to the directory where you wish to complete theexercise. If needed, use the cd command in Octave/MATLAB to change tothis directory before starting this exercise.You can also find instructions for installing Octave/MATLAB in the “En&#x2;vironment Setup Instructions” of the course website.Files included in this exerciseex1.m – Octave/MATLAB script that steps you through the exerciseex1 multi.m – Octave/MATLAB script for the later parts of the exerciseex1data1.txt – Dataset for linear regression with one variableex1data2.txt – Dataset for linear regression with multiple variablessubmit.m – Submission script that sends your solutions to our servers[?] warmUpExercise.m – Simple example function in Octave/MATLAB[?] plotData.m – Function to display the dataset[?] computeCost.m – Function to compute the cost of linear regression[?] gradientDescent.m – Function to run gradient descent[†] computeCostMulti.m – Cost function for multiple variables[†] gradientDescentMulti.m – Gradient descent for multiple variables[†] featureNormalize.m – Function to normalize features[†] normalEqn.m – Function to compute the normal equations? indicates files you will need to complete† indicates optional exercises1Throughout the exercise, you will be using the scripts ex1.m and ex1 multi.m.These scripts set up the dataset for the problems and make calls to functionsthat you will write. You do not need to modify either of them. You are onlyrequired to modify functions in other files, by following the instructions inthis assignment.For this programming exercise, you are only required to complete the firstpart of the exercise to implement linear regression with one variable. Thesecond part of the exercise, which is optional, covers linear regression withmultiple variables.Where to get helpThe exercises in this course use Octave1 or MATLAB, a high-level program&#x2;ming language well-suited for numerical computations. If you do not haveOctave or MATLAB installed, please refer to the installation instructions inthe “Environment Setup Instructions” of the course website.At the Octave/MATLAB command line, typing help followed by a func&#x2;tion name displays documentation for a built-in function. For example, helpplot will bring up help information for plotting. Further documentation forOctave functions can be found at the Octave documentation pages. MAT&#x2;LAB documentation can be found at the MATLAB documentation pages.We also strongly encourage using the online Discussions to discuss ex&#x2;ercises with other students. However, do not look at any source code writtenby others or share your source code with others.1 Simple Octave/MATLAB functionThe first part of ex1.m gives you practice with Octave/MATLAB syntax andthe homework submission process. In the file warmUpExercise.m, you willfind the outline of an Octave/MATLAB function. Modify it to return a 5 x5 identity matrix by filling in the following code:A = eye(5);1Octave is a free alternative to MATLAB. For the programming exercises, you are freeto use either Octave or MATLAB.2When you are finished, run ex1.m (assuming you are in the correct di&#x2;rectory, type “ex1” at the Octave/MATLAB prompt) and you should seeoutput similar to the following:ans =Diagonal Matrix1 0 0 0 00 1 0 0 00 0 1 0 00 0 0 1 00 0 0 0 1Now ex1.m will pause until you press any key, and then will run the codefor the next part of the assignment. If you wish to quit, typing ctrl-c willstop the program in the middle of its run.1.1 Submitting SolutionsAfter completing a part of the exercise, you can submit your solutions forgrading by typing submit at the Octave/MATLAB command line. The sub&#x2;mission script will prompt you for your login e-mail and submission tokenand ask you which files you want to submit. You can obtain a submissiontoken from the web page for the assignment.You should now submit your solutions.You are allowed to submit your solutions multiple times, and we will takeonly the highest score into consideration.2 Linear regression with one variableIn this part of this exercise, you will implement linear regression with onevariable to predict profits for a food truck. Suppose you are the CEO of arestaurant franchise and are considering different cities for opening a newoutlet. The chain already has trucks in various cities and you have data forprofits and populations from the cities.3You would like to use this data to help you select which city to expandto next.The file ex1data1.txt contains the dataset for our linear regression prob&#x2;lem. The first column is the population of a city and the second column isthe profit of a food truck in that city. A negative value for profit indicates aloss.The ex1.m script has already been set up to load this data for you.2.1 Plotting the DataBefore starting on any task, it is often useful to understand the data byvisualizing it. For this dataset, you can use a scatter plot to visualize thedata, since it has only two properties to plot (profit and population). (Manyother problems that you will encounter in real life are multi-dimensional andcan’t be plotted on a 2-d plot.)In ex1.m, the dataset is loaded from the data file into the variables Xand y:data = load(‘ex1data1.txt’); % read comma separated dataX = data(:, 1); y = data(:, 2);m = length(y); % number of training examplesNext, the script calls the plotData function to create a scatter plot ofthe data. Your job is to complete plotData.m to draw the plot; modify thefile and fill in the following code:plot(x, y, ‘rx’, ‘MarkerSize’, 10); % Plot the dataylabel(‘Profit in $10,000s’); % Set the y−axis labelxlabel(‘Population of City in 10,000s’); % Set the x−axis labelNow, when you continue to run ex1.m, our end result should look likeFigure 1, with the same red “x” markers and axis labels.To learn more about the plot command, you can type help plot at theOctave/MATLAB command prompt or to search online for plotting doc&#x2;umentation. (To change the markers to red “x”, we used the option ‘rx’together with the plot command, i.e., plot(..,[your options here],..,‘rx’); )44 6 8 10 12 14 16 18 20 22 24−50510152025Profit in $10,000sPopulation of City in 10,000sFigure 1: Scatter plot of training data2.2 Gradient DescentIn this part, you will fit the linear regression parameters θ to our datasetusing gradient descent.2.2.1 Update EquationsThe objective of linear regression is to minimize the cost functionJ(θ) = 12mXmi=1