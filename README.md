# Introduction
In this project, we utilize the Smith-Waterman algorithm to implement dynamic programming for local pairwise sequence alignment. We establish a matrix of zeros that get updated as the score is calculated, determine the optimal traceback direction based on the highest scores, and then perform local alignment. Local alignment differs from global alignment in that the all negative scores get reset to zero. This is because we are not trying to align the full sequence, only regions, so negative scores will not accumulate through regions of dissimilarity. Our expected output is a matrix of scores and the best alignment for two sequences. 

# Dependencies
numpy

# Pseudocode

```
Global variables for directionality:
END = 0
DIAG = 1
UP = 2
LEFT = 3

cal_score():
  Compare sequence 1 and 2
    if the seqeunces are the same:
      diagonal score is a match
    otherwise:
      diagonal scores are a mismatch
  
  calculate gap scores for up and left
  
  set the best score equal to the max of diagonal, up, and left scores, setting the lower bound to 0
  
  Determining the traceback direction
    if the best score is 0--> end
  Determining tie-breaker preference using pythons top down variable convention (Diagonal then up then left)
      if the best score is diagonal --> diagonal
      if the best score is up --> up
      otherwise --> left
  Return the score and direction


traceback():
initialize empty lists to hold the two sequences
start at maximum position for row and column

while true that condition is not end
  current_move = traceback_matrix[current_row][current_col] # line given in template
  if the current move is the end
    break
  if current_move == DIAG:
      append the row and column to the traceback sequence
      decrease current row/column by one to move backwards
  elif current_move == UP:
      append the row to the respective traceback sequence
      add a gap to the column sequence
      decrease row count by 1 to move backward
  elif current_move == LEFT:
      append the column to the respective traceback sequence
      add a gap to the row sequence
      decrease column count by 1 to move backward
  join and reverse both seqeunces to get final aligned sequences 1 and 2
  return both aligned sequences


smith-waterman():
  Initialize variables for the length of seq1 and 2
  initialize starting count and score for highest score and position
  Initialize scoring matrix H with zeros using numpy, specify integer to match expected output
  Initialize a traceback matrix for the directionality
    For i from 1 to m:
        For j from 1 to n:
          call cal_score for best score and direction using the sscore matrix, both sequences, i, j, and the scoring calculations
          assign best score and direction into the two matrices
          if the best score is greater than the highest score
            the highest score is now the best score
            the highest position is at positions i and j
    
    Call traceback() and put into the aligned sequences
    Return the optimal local alignment

```

# Output and Reasoning 
Output from first driver:
```
AC-TTA
ACATTA
[[0 0 0 0 0 0 0 0 0]
 [0 0 0 0 0 1 1 0 0]
 [0 0 1 0 1 0 0 2 1]
 [0 1 0 2 1 0 0 1 1]
 [0 0 0 1 1 2 1 0 0]
 [0 0 0 0 0 2 3 2 1]
 [0 0 1 0 1 1 2 4 3]
 [0 0 0 0 0 0 1 3 3]]
```
Output from second driver:
```
GGGGAAAAAA
GGGGAAAAAA
[[ 0  0  0  0  0  0  0  0  0  0  0  0  0  0  0]
 [ 0  1  1  1  1  0  0  0  0  0  0  0  0  0  0]
 [ 0  1  2  2  2  1  0  0  0  0  0  0  0  0  0]
 [ 0  1  2  3  3  2  1  0  0  0  0  0  0  0  0]
 [ 0  1  2  3  4  3  2  1  0  0  0  0  0  0  0]
 [ 0  0  1  2  3  5  4  3  2  1  1  0  0  0  0]
 [ 0  0  0  1  2  4  6  5  4  3  2  1  0  0  0]
 [ 0  0  0  0  1  3  5  7  6  5  4  3  2  1  0]
 [ 0  0  0  0  0  2  4  6  8  7  6  5  4  3  2]
 [ 0  0  0  0  0  1  3  5  7  9  8  7  6  5  4]
 [ 0  0  0  0  0  1  2  4  6  8 10  9  8  7  6]
 [ 0  0  0  0  0  1  2  3  5  7  9  9  8  7  6]
 [ 0  0  0  0  0  1  2  3  4  6  8  8  8  7  6]
 [ 0  0  0  0  0  1  2  3  4  5  7  7  7  7  6]
 [ 0  0  0  0  0  1  2  3  4  5  6  6  6  6  6]
 [ 0  0  0  0  0  1  2  3  4  5  6  5  5  5  5]
 [ 0  0  0  0  0  1  2  3  4  5  6  5  4  4  4]
 [ 0  0  0  0  0  0  1  2  3  4  5  7  6  5  5]
 [ 0  0  0  0  0  0  0  1  2  3  4  6  8  7  6]
 [ 0  0  0  0  0  0  0  0  1  2  3  5  7  9  8]
 [ 0  0  0  0  0  0  0  0  0  1  2  4  6  8 10]]
```

Here we elected to only return a single local optimal alignment, rather than store and return all maximum local alignments, due to time constraints. 

# Successes
The logic flow of this project was much more straightforward than other projects.
We were able to navigate conflicting schedules, each group member was able to implement all three functions independently without major problems, and our individual versions all produced the expected output for both drivers. Our team was able to then work efficiently to compare approaches rather than spending time debugging to determine the best final version and documentation.


# Struggles
Despite all three of us coming up with similar code, we found there could still be improvements. For example, our code does not return multiple alignments if there are equivalent “bests”. This is something that we can see being common in real biological data, and it is also seen in the last sequence alignment task of this project. On the surface, this would be straightforward to add, calling traceback for each maximum in the graph, however, it gets more complicated when considering that each maximum could have multiple traceback paths. This is something we would have attempted given more time.

One struggle that arose throughout the coding process was understanding the logic of the traceback through code, such as which sequence gets the gap depending on direction. For example, which of the two sequences receives “-“ if the traceback direction is “up”. Some things that helped with this were trial and error, thinking through what was changed during the movement, and adding clear comments to keep track of which sequences are changing in order to prevent confusion. Lastly, figuring out we needed an extra row and column of 0s, and then managing the indexing of this matrix vs the starting sequences was a further challenge.  

# Personal Reflections
## Group Leader
Mara:
This project was much more straightforward that the previous two. I am also taking Genomics alongside this class, and we recently discussed dynamic programming and the Smith-Waterman algorithm, so I was already acquainted with the principle and it was interesting getting to implement it. I found the coding of the traceback to be the most difficult portion. Because we decided to only return a single maximum alignment, we implemented a tie-breaker in the order of diagonal > up > left. I would have liked to return all optimal alignments, and had starting brainstorming how I would do that, but ultimately realized that it would require too many code changes for what our group had time left to do, and we all decided against it. If I were to improve on this code later on, I would try to add that.  

My collaborators and I worked well together. We had some conflicting schedules, so took the approach of code independently and compare later. We checked in with each other regularly to ensure we were all on approximately the same step around the same time, that nobody was getting lost, and that all decision-making would be done together, such as with the tie-breaker and returning a single alignment. We took the steps necessary to ensure efficient communication, and no single person was overruled in the final version, which ended up working quite well for us. I also found being group leader again to be even easier than the first couple times. 

## Collaborators
##### Katelyn: 

I also took Genomics last semester, where we covered local alignment and the Smith-Waterman algorithm, so I was familiar with how the scoring matrix and traceback were going to work for this project. It was good practice implementing it in code again because it made me think about how to translate the process to something that the program can follow. Writing the code went mostly smoothly, but I did think the traceback was the hardest part because it had to both keep track of the position and build the sequences backward. Everyone in the group was able to finish the functions independently without major problems, so our final call was mostly spent comparing approaches and finalizing one version. Our implementation returns a single optimal alignment, since it takes the first maximal cell and breaks ties diagonal, then up, then left, but the second driver program has 2 alignments with the same top score and only one is reported. We discussed if we should make it return all the alignments or just one of them, and ultimately decided that we would just return one since that's what the assignment asked for. If we were to improve this program, we would try to find a way to have it return all the alignments with the same top score.

##### Matthew:

I found this project to be much more digestible than sequence assembly. Additionally, developing a general understanding of how projects tend to be structured also likely aided in this. For instance, writing one "main" function that calls the other functions. On this idea, something I could improve on is more concretely understanding how each function fits into the bigger picture before writing code/pseudocode. Sometimes I will have a good idea of what a function should do based on the name and my understanding of the project but have to rework/rethink after looking closer at the arguments and returns specified in the assignment. 

Despite being less conceptually difficult than last week. It still took some mental effort to figure out how each move in the traceback should translate to code. Unlike doing it by hand in class, the good thing about code is you only had to figure it out once. It also took looking at some of the class readings (Smith & Waterman, 1981) and expected outputs in the assignment to figure out I needed to add an extra row and column of 0s.

Finally, I thought our group worked very well together. We found a couple times to meet but all worked on code relatively independently. At the end we compared our results and found we had similar implementations.

# Generative AI Appendix
As per the syllabus

Katelyn: I used Claude Opus 5.5 to first give me a refresher on how the Smith-Waterman algorithm fills the scoring matrix and performs the traceback. I also used it to help when I realized I was having a problem with indexing, and it helped me understand the index offset between the matrix and the sequences. I wrote the pseudocode and code for all 3 functions myself, and Claude guided me and reviewed them for errors, pointing out syntax mistakes, suggested I use string concatenation rather than .append, and that we needed to add the negative values. I also used a suggested line of code that used numpy to find the maximum position using np.unravel_index with np.argmax, but we ultimately didn't use this in our final version. I also had it explain to me what to do about ties, and used it to help me with Git and Github. Finally, I used it to help me brainstorm ideas for my reflection.

# Additional Resources
Smith, T. F., & Waterman, M. S. (1981). Identification of common molecular subsequences. Journal of Molecular Biology, 147(1), 195–197. https://doi.org/10.1016/0022-2836(81)90087-5

