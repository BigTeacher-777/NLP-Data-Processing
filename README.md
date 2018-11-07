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
| pad_data | pad all sentences in data |
| emb_dim  | Embeddings dimension |
| emb_path | the path to the word embeddings file |

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

## Create data
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

## Pad data
**pad all sentences to the maximum length max_len**
```
pad_data = []
for d in data:
    tmp = np.pad(d,(max_len-len(d),0),'constant',constant_values=(0))
    pad_data.append(tmp)
pad_data = np.array(pad_data)

return pad_data
```

## Word Embedding
**Word2vec**
```
from gensim.models.keyedvectors import KeyedVectors

self.embedding = nn.Embedding(vocab_size,emb_dim)
word_vectors = KeyedVectors.load_word2vec_format(emb_path)\
matrix = []
for i in range(len(vocab)):
   word = vocab.keys()[i]
   if word in word_vectors.vocab:
      matrix.append(word_vectors.word_vec(word))
   else:
      matrix.append(np.random.uniform(-0.01,0.01,300).astype('float32'))
matrix = np.array(matrix)
self.embedding.weight.data.copy_(torch.from_numpy(matrix))
```

**Glove:need to process txt before gensim load**
```
f = open(emb_path,'r')
count = 0
for line in f:
   count += 1
genism_first_line = "{} {}".format(count,emb_dim)
with open(emb_path,'r') as old:
   with open(new_emb_path,'w') as new:
      new.write(str(gensim_first_line)+'\n')
      shutil.copyfileobj(old,new)
return new_emb_path
#the next process is the same as Word2vec
```





