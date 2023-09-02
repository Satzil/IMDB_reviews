#  Sentiment analysis using Natural Language Processing

![enter image description here](https://github.com/Satzil/IMDB_reviews/blob/main/images/vectors.png?raw=true)

## Introduction

Sentiment analysis, also known as opinion mining, is a Natural Language Processing (NLP) technique that involves the use of computational methods and algorithms to automatically determine and categorize the sentiment or emotional tone expressed in a piece of text, such as a sentence, paragraph, or document. The primary goal of sentiment analysis is to understand whether the sentiment expressed in the text is positive, negative, neutral, or sometimes more nuanced emotions like joy, anger, sadness, etc.

Sentiment analysis on IMDb reviews involves using NLP techniques to automatically classify the sentiments expressed in the movie reviews available on the IMDb platform. The goal is to determine whether a given movie review is positive or negative based on the text content. Here's how sentiment analysis can be applied to IMDb reviews:

## Data Collection
 
To work with IMDb movie reviews using TensorFlow and its datasets module, you can utilize the "imdb_reviews" dataset from the "tensorflow_datasets" module, which is a commonly used dataset for sentiment analysis. This dataset contains movie reviews labeled as positive or negative.

    import tensorflow_datasets as tfds
	imdb, info = tfds.load("imdb_reviews", with_info = True, as_supervised = True)

## Text Preprocessing

Text data is typically raw and unstructured, making it challenging to work with directly. Tokenization is the first step in preprocessing text, breaking it down into smaller, more manageable units.

Tokenization allows you to convert text data into a format that machine learning models can understand. In NLP tasks, text is often represented as numerical features, with each token (word or subword) corresponding to a unique numerical index. It helps computers understand the structure of language, including sentence and word boundaries, which is crucial for parsing and interpreting text correctly.

**Mapping Words to Indices:** A word index is essentially a lookup table where each word in a given vocabulary is associated with a unique integer index. For example:

    {'the': 1, 'cat': 2, 'sat': 3, 'on': 4, 'mat': 5}

To create a word index, you typically start with a corpus of text data and scan through it to identify all unique words or tokens. Each unique word is assigned an index in the dictionary-like structure. The vocabulary size is the total number of unique words in the corpus.

**Mapping Text to Integers:** As you iterate through the tokens in the text, you replace each token with its integer index based on the word index lookup. This process results in a sequence of integers that represent the original text.

    text_data = ["This is an example sentence.", "Another sentence for tokenization."]

**Word Index (generated by the tokenizer):**

    {
	    'sentence': 1,
	    'this': 2,
	    'is': 3,
	    'an': 4,
	    'example': 5,
	    'another': 6,
	    'for': 7,
	    'tokenization': 8
	}

**Text Sequences:**

    [
	    [2, 3, 4, 5, 1],
	    [6, 1, 7, 8]
	]
	

**Padded Sequences:** After converting text to sequences of integers, it's common to apply padding to these sequences to ensure that they all have the same length. Padding is important when you're working with sequences because many machine-learning models expect input data of the same shape.

    [
	    [2, 3, 4, 5, 1, 0, 0],  # Padding added to reach a fixed length
	    [6, 1, 7, 8, 0, 0, 0]   # Padding added to reach a fixed length
	]

## Embedding

In natural language processing (NLP), an "embedding" refers to the representation of words or tokens as dense vectors in a continuous, lower-dimensional space. Word embeddings are designed to capture semantic relationships between words and are widely used in various NLP tasks.

Each word or token in a vocabulary is represented as a fixed-size vector, typically with a length (dimensionality) of 50, 100, 200, or more. These vectors are often real-valued numbers. Word embeddings are learned from large corpora of text data, and the vectors are designed to capture the semantic meaning of words. Words with similar meanings or contexts tend to have similar vector representations.

In deep learning models for NLP, an "embedding layer" is often added at the beginning of the model. This layer maps words from the vocabulary to their corresponding word embeddings. During training, the embedding weights are adjusted to minimize the model's loss.

    model = tf.keras.Sequential([
	    tf.keras.layers.Embedding(vocab_size, embedding_dim, input_length=max_length),
	    tf.keras.layers.Flatten(), 
	    tf.keras.layers.Dense(6, activation='relu'), 
	    tf.keras.layers.Dense (1, activation='sigmoid')
	])
Embedding weights of the first five vectors trained on "imdb_reviews" dataset.

    [[-0.09154893  0.02042527  0.03338276  0.00214122  0.02543209 -0.01413436 0.02098971 -0.00164257  0.00291977  0.00122652  0.00103333 -0.0166014 -0.00452532  0.00694443  0.03660228  0.00842354]
	 [-0.07992429  0.04919256  0.02199429 -0.03495118 -0.0049047  -0.09176634 -0.02686577  0.03442561  0.01560283 -0.03198145 -0.00568978 -0.00616419 -0.02165268 -0.07788812  0.09566456  0.07579771]
	 [-0.08534582  0.01814275 -0.01686682 -0.0585026  -0.0240049  -0.07957026 0.0104718  -0.01774652 -0.01540054  0.02612891  0.02775123 -0.00141767 -0.01899928 -0.00106876  0.07749649  0.04836336]
	 [-0.0285248   0.01430658  0.04745916 -0.02150241  0.00734093 -0.06749713 0.03226088  0.0410864   0.07156884 -0.06943589  0.07537635 -0.03245897 -0.04960522 -0.01062288  0.01141625  0.06392775]
	 [-0.09380264  0.03648637 -0.02789726 -0.05364898 -0.01220282 -0.07093333 0.01232904  0.05437711  0.04350393  0.00487533 -0.01263185  0.00078373 0.01179482  0.03103561  0.00344059  0.01753972]]
Embeddings are dense, real-valued vectors that represent words or tokens from a vocabulary. Each word or token is associated with a unique embedding vector. These embeddings are learned during the training of the neural network and are capable of capturing semantic relationships between words.

## Analysis of the model

The model is trained on the dataset that has been preprocessed from the previous steps and validated against the validation set. 

> Accuracy of the model on "imdb_reviews" dataset

![enter image description here](https://github.com/Satzil/IMDB_reviews/blob/main/images/accuracy_imdb.png?raw=true)

> loss of the model on "imdb_reviews" dataset

![enter image description here](https://github.com/Satzil/IMDB_reviews/blob/main/images/loss_imdb.png?raw=true)

Judging by the results the model is said to be overfitted. The accuracy of the validation set constantly decreases with an increase in iterations.
Since the model is overfitted the LSTM approach is used to solve the issue. 

## LSTM(Long Short-Term Memory)

Long Short-Term Memory (LSTM) is a type of recurrent neural network (RNN) architecture that has been widely used in Natural Language Processing (NLP) tasks. LSTMs are designed to handle sequential data and have proven to be very effective in various NLP applications due to their ability to capture long-range dependencies in text data.

LSTMs are capable of learning and remembering information over long time intervals. They have a mechanism for selectively retaining information from the past and updating it with new information. This is crucial for NLP tasks where understanding context often requires referencing words or phrases from earlier in a document or conversation.

    model = tf.keras.Sequential([
	    layers.Embedding(vocab_size, 64),
	    layers.Bidirectional(layers.LSTM(64 , return_sequences = True)),
	    layers.Bidirectional(layers.LSTM(32)),
	    layers.Dense(64, activation = 'relu'),
	    layers.Dense(1, activation = 'sigmoid')
	])
Adding an LSTM layer to a deep learning model involves using a deep learning framework such as TensorFlow. It is simply added as a layer after the Embedding layer. Multiple LSTM layers can be stacked together for better results. Bidirectional LSTM is used in this model.

Unlike traditional LSTMs that process sequences in a unidirectional manner (from past to future), BiLSTMs process sequences in both directions (from past to future and from future to past). This bidirectional processing allows them to capture information and dependencies from both the left and right contexts of each time step in the sequence, which can be valuable for understanding the overall context of the data.

## Analysis of the LSTM model

> Accuracy of the LSTM model

![enter image description here](https://github.com/Satzil/IMDB_reviews/blob/main/images/accuracy_lstm_imdb.png?raw=true)

> Loss of the LSTM model

![enter image description here](https://github.com/Satzil/IMDB_reviews/blob/main/images/loss_lstm_imdb.png?raw=true)

LSTM models seemed to have worse results than the traditional model.
The accuracy was not quite good and the loss of the model was much higher.

The performance of a machine learning or deep learning model, including LSTM, can vary depending on the dataset and the specific characteristics of the data. It's not uncommon for an LSTM to perform differently on different datasets.

##   8k subwords IMDb dataset

The "8k subwords IMDb dataset" likely refers to a modified version of the IMDb (Internet Movie Database) dataset for sentiment analysis, where the text data has been preprocessed by tokenizing it into subword units using techniques like Byte-Pair Encoding (BPE) or SentencePiece. This preprocessing is commonly used to handle out-of-vocabulary (OOV) words and provide a finer-grained representation of text.

The "8k subwords" part indicates that instead of using traditional word-level tokenization, the text data in this dataset has been tokenized into subword units. Subword tokenization is a technique that breaks text into smaller units, such as subword pieces or characters. The "8k" suggests that there are approximately 8,000 subword tokens in the vocabulary.

    ['the_', ', ', '. ', 'a_', 'and_', 'of_', 'to_', 's_', 'is_', 'br', 'in_', 'I_', 'that_', 'this_', 'it_', ' /><', ' />', 'was_', 'The_', 'as_', 't_', 'with_', 'for_', '.<', 'on_', 'but_', 'movie_', ' (', 'are_', 'his_', 'have_', 'film_', 'not_', 'ing_', 'be_', 'ed_', 'you_', ' "', 'it', 'd_', 'an_', 'he_', 'by_', 'at_', 'one_', 'who_', 'y_', 'from_', 'e_', 'or_', 'all_', 'like_', 'they_', '" ', 'so_', 'just_', 'has_', ') ', 'her_', 'about_', 'out_', 'This_', 'some_', 'ly_', 'movie', 'film', 'very_', 'more_', 'It_', 'would_', 'what_', 'when_', 'which_', 'good_', 'if_', 'up_', 'only_', 'even_', 'their_', 'had_', 'really_', 'my_', 'can_', 'no_', 'were_', 'see_', 'she_', '? ', 'than_', '! ', 'there_', 'get_', 'been_', 'into_', ' - ', 'will_', 'much_', 'story_', 'because_', 'ing', 'time_', 'n_', 'we_', 'ed', 'me_', ': ', 'most_', 'other_', 'don', 'do_', 'm_', 'es_', 'how_', 'also_', 'make_', 'its_', 'could_', 'first_', 'any_', "' ", 'people_', 'great_', 've_', 'ly', 'er_', 'made_', 'r_', 'But_', 'think_', " '", 'i_', 'bad_', 'A_', 'And_', 'It', 'on', '; ', 'him_', 'being_', 'never_', 'way_', 'that', 'many_', 'then_', 'where_', 'two_', 'In_', 'after_', 'too_', 'little_', 'you', '), ', 'well_', 'ng_', 'your_', 'If_', 'l_', '). ', 'does_', 'ever_', 'them_', 'did_', 'watch_', 'know_', 'seen_', 'time', 'er', 'character_', 'over_', 'characters_', 'movies_', 'man_', 'There_', 'love_', 'best_', 'still_', 'off_', 'such_', 'in', 'should_', 'the', 're_', 'He_', 'plot_', 'films_', 'go_', 'these_', 'acting_', 'doesn', 'es', 'show_', 'through_', 'better_', 'al_', 'something_', 'didn', 'back_', 'those_', 'us_', 'less_', '...', 'say_', 'is', 'one', 'makes_', 'and', 'can', 'all', 'ion_', 'find_', 'scene_', 'old_', 'real_', 'few_', 'going_', 'well', 'actually_', 'watching_', 'life_', 'me', '. <', 'o_', 'man', 'there', 'scenes_', 'same_', 'he', 'end_', 'this', '... ', 'k_', 'while_', 'thing_', 'of', 'look_', 'quite_', 'out', 'lot_', 'want_', 'why_', 'seems_', 'every_', 'll_', 'pretty_', 'got_', 'able_', 'nothing_', 'good']

These are some of the subwords that are tokenized from the "imdb_reviews/subwords8k" dataset.

## Analysis of the 8k subwords model

The same model was used to train the neural networks on "imdb_reviews/subwords8k" dataset and the results were pretty good than the previous models.

![enter image description here](https://github.com/Satzil/IMDB_reviews/blob/main/images/accuracy_lstm_8kimdb.png?raw=true)

![enter image description here](https://github.com/Satzil/IMDB_reviews/blob/main/images/loss_lstm_8kimdb.png?raw=true)

The performance of the model which is trained on "8k subwords imdb dataset" seemed to show better results than the model which is trained on the normal "imdb reviews" dataset. The accuracy of the validation set turned out to be better and has shown a slight improvement compared to "imdb reviews" dataset.
 
 ## Resources

[IMDb LSTM reviews notebook](https://www.kaggle.com/code/satzil/imdb-lstm)

[IMDb reviews notebook](https://www.kaggle.com/code/satzil/imdbreviews)








