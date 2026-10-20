# Introduction
In this project, we utilize the Smith-Waterman algorithm to implement dynamic programming for local pairwise sequence alignment. 

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

Here we elected to only return a single local optimal alignment, rather than store and return all maximum local alignments. 

# Successes
Each group member was able to implement all three functions independently without major problems, and our individual versions all produced the expected output for both drivers. Our team was able to then work efficiently to compare approaches rather than spending time debugging.

# Struggles
Talking points:

Understanding the logic of the traceback through code

keeping track of what variable was associated with what position

understanding that there was an index offset between the matrix and the sequences because of the extra row and column of zeros




(Reflection narrative that describes what went right, what went wrong, what you didn't understand, how it could have been better, and how you could have been better.)
# Personal Reflections
## Group Leader
Mara:
This project was much more straightforward that the previous two. I am also taking Genomics alongside this class, and we recently discussed dynamic programming and the Smith-Waterman algorithm, so I was already acquainted with the principle, so it was interesting getting to implement it. 
My collaborators and I worked well together. 

## Collaborators
Other members' reflections on the project
Katelyn: I also took Genomics last semester, where we covered local alignment and the Smith-Waterman algorithm, so I was familiar with how the scoring matrix and traceback were going to work for this project. It was good practice implementing it in code again because it made me think about how to translate the process to something that the program can follow. Writing the code went mostly smoothly, but I did think the traceback was the hardest part because it had to both keep track of the position and build the sequences backward. Everyone in the group was able to finish the functions independently without major problems, so our final call was mostly spent comparing approaches and finalizing one version. Our implementation returns a single optimal alignment, since it takes the first maximal cell and breaks ties diagonal, then up, then left, but the second driver program has 2 alignments with the same top score and only one is reported. We discussed if we should make it return all the alignments or just one of them, and ultimately decided that we would just return one since that's what the assignment asked for. If we were to improve this program, we would try to find a way to have it return all the alignments with the same top score.
# Generative AI Appendix
As per the syllabus

Katelyn: I used Claude Opus 5.5 to first give me a refresher on how the Smith-Waterman algorithm fills the scoring matrix and performs the traceback. I also used it to help when I realized I was having a problem with indexing, and it helped me understand the index offset between the matrix and the sequences. I wrote the pseudocode and code for all 3 functions myself, and Claude guided me and reviewed them for errors, pointing out syntax mistakes, suggested I use string concatenation rather than .append, and that we needed to add the negative values. I also used a suggested line of code that used numpy to find the maximum position using np.unravel_index with np.argmax, but we ultimately didn't use this in our final version. I also had it explain to me what to do about ties, and used it to help me with Git and Github. Finally, I used it to help me brainstorm ideas for my reflection.