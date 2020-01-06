# Horizon finding

The underlying theme for the problem statement requires the use of Bayes Net and Viterbi Algorithm for finding the Horizon of sample images. An image, which can be seen as a matrix of rows and columns has an image gradient at each cell. Keeping this mind, we
must estimate the most fitting row value for each column in the image pixels. The gradient data becomes our observed values which are the vectorized column values and
the row data is the hidden variables value at each point.


## Bayes Net

In this part we simply pick the maximum edge strength in each column. Using variable elimination we find that the the pixel only depends on the column it is present in.

`s * i = arg max P(Si = si|w1, ..., wm)`

The results of this method is shown below,

![alt text](attachments/output_simple.jpg)
![alt text](attachments/output_simple1.jpg)

##  Viterbi Algorithm

This subset uses the Viterbi algorithm to find an optimal solution or rather the most likely sequence for a set of given states. The Viterbi function taken in the following:
The initial probability which is 1, the emission probabilities and the transition probabilities.
In this case we try to maximise the posterior. The posterior is given as:

```python
P(S1 = s1, ..., Sm = sm|w1, ..., wm) = P(w1, ..., w = wm|S1..., sm=Sm) * P(S+1|S) * (W0) // Normalising factor
```

Since the denominator is same for all states we only maximise the numerator.

Maximising a function or log of the function is the same thing. Here we are maximising log posterior.
hence,

`LOG P(S1 = s1, ..., Sm = sm|w1, ..., wm) = Π LOG P(Wn|sn) + Π LOG P(S+1|S) + log(W0)`

- **Emission** : For each column,  for each row (Pixel strength/Σpixel strength)
- **Transition** : The transition probability from one state to other state only depends on the previous column. The transition probability is high when row difference is less between the columns and low if row difference is high. Thus as we iterate through the column list, values nearby have a higher probability of converging as the maximum value of the transition function.The key concept behind this strategy is that horizon is always smooth and hence we don't want any jumps between the rows.<br/>
The probability we made is - *no. of rows-row_difference / no. of rows.*<br/>

   This probability satisfies `Σ transition prob = 1` as it is also normalised and is consistent with the logic stated above.<br/>

   Choice value = *log(Initial Probability) + log(Transition Probability) + log(Emission Probability)*<br/>

   The choice of our path depends on the maximum value received through taking the choice value of every column, we pick the larger of the  competing values and record the higher probability path.

Thus for any point N, we consider the most strategic path or rather maximum probavilty upto path N-1.

![alt text](attachments/output_map.jpg)
![alt text](attachments/output_map1.jpg)

##  Bi-directional Viterbi Algorithm

The third problem statement asserts the need of human intervention for feedback. We can execute this function by carrying out a bidirectional search Viterbi Algorithm.
This means that the Viterbi algorithm runs for the given input row and column position and visits the neighbouring elements on either of its sides. This strategic approach ensures we append the max array value which is at a specific distance from the given feedback.

![alt text](attachments/output_human.jpg)
![alt text](attachments/output_human1.jpg)

We have given coordinates which lie on horizon as input and we run bidirectional Viterbi. For example the coordinates are 78,7 the forward pass runs from 8 to last column and the backward pass runs from 6 to column0. We store the output of both and add in one array. The output of backward pass needs to be reversed before combining. We then put pixel on the combined output.

## How To Run?

Below are the instructions to run the above code. The images are present in the folder **test_images**. If you want to try out new images, just put those images in that folder.

1. `git clone https://github.com/hrishikeshpaul/horizon-detection.git`
2. `cd horizon-detection`
3. `python3 mountain.py mountain1.jpg 60 180`
