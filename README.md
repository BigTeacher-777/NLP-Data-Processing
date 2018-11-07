NLP Data Processing
==

I am recently working on sentiment analysis and encountered many difficulties in data processing and word embedding. Due to the chaos of online tutorials, I make a systematic summary on how to solve the above difficulties.



## Variable Define & Setting
| Variable | Define & Setting | 
| :----:   | :----: |
|data_file | the name of the dataset |
|  vocab   | vocabulary containing all used words in the dataset |
|vocab_size| the size of vocab |
| max_len  | the maximum length of sentences in the dataset |
|  data    | the dataset converted to index of vocabulary |

## Create vocabulary
**Collect all used words in the dataset in one vocabulary**
```Shell
import codecs
import operator
import re

num_regex = re.compile('^[+-]?[0-9]+\.?[0-9]*$')
file = codecs.open(data_file,'r','utf-8')
word_freqs = {}  # record words and their frequency
for line in file:
   words = line.split()
   for word in words:
      if not bool(num_regrex.match(word)):
         try:
            word_freqs[word] += 1
  	 except KeyError:
	    word_freqs[word] = 1
   # sorted words in descending order based on frequency
   sorted_word_freqs = sorted(word_freqs.items(), key=operator.itemgetter(1), reverse=True)
   vocab = {'<pad>':0, '<unk>':1, '<num>':2}
   index = len(vocab)
   for word, _ in sorted_word_freqs:
      vocab[word] = index
      index += 1

return vocab

```

## create data
**transform words in the dataset into the index of vocabulary**
```
file = codecs.open(data_file,'r','utf-8')
data = []
max_len = 0
for line in file:
   word_indices = []
   words = line.split()
   for word in words:
      if bool(num_regex.match(word)):
         word_indices.append(vocab['<num>'])
         num_hit += 1
      elif word in vocab:
         word_ind = vocab[word]
         word_indices.append(word_ind)
      else:
         word_indices.append(vocab['<unk>'])
      if len(word_indices) > max_len:
         max_len = len(word_indices)
   data.append(word_indices)
data = np.array(data)

return data,max_len
```




