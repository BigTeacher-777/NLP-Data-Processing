NLP Data Processing
==

I am recently working on sentiment analysis and encountered many difficulties in data processing and word embedding. Due to the chaos of online tutorials, I make a systematic summary on how to solve the above difficulties.



## Variable Define & Setting
data_file  ------ the name of the dataset
    vocab  ------ vocabulary containing all used words in the dataset
vocab_size ------ the size of vocab

## Create vocabulary
Collect all used words in the dataset in one vocabulary
```Shell
import codecs

num_regex = re.compile('^[+-]?[0-9]+\.?[0-9]*$')
file = codecs.open(data_file,'r','utf-8')
for line in file:
   words = line.split()
   for word in words:
      if not bool(num_regrex.match(word)):
         try:
            word
```

