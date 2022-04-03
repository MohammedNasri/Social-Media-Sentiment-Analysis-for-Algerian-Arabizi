This repo contains my approach in solving the Algerian dialect sentiment analysis problem. 

The competition posed several challenges:

1- Data collection of Algerian dialect "Arabizi" from social media using web scraping.

2- Algerian dialect has a high degree of variance.

3- No available pretrained models for Algerian dialect nor any Maghrebi dialect.

4- The text is in latin characters (Arabizi) not Arabic.
 
 #Solution
 
If we analyze the Algerian dialect, we would find that it is a melting pot of many languages: Arabic by a big margin (root of words, grammar, etc ...), Amazigh words (which is shared among all Maghrebi dialects), French (also shared with some Maghrebi dialects), and to a certain extent some English and Italian.

Because of this diversity, using an ensemble of multiple pretrained language models fine-tuned on the same dataset would surely yield higher accuracy since every model would contribute to the language understanding by a bit. But how is it possible to take advantage of Arabic pretrained model and the dataset is in latin letters, one might ask.

For that reason, I trained an independent transformer model for transliterating from Latin letters to Algerian using a dataset that I personally harvested and annotated. This dataset contains around 17k commonly used Algerian words in both Arabic letters and Arabizi. More on the dataset later.

After trying several pretrained models from the huggingface hub, and through lots trial and error, I determined that the combination of bert-base-uncased, alongside with moha/arabert_c19 (multi-dialect Arabic model trained on 1.5M COVID19 tweets, paper: https://arxiv.org/pdf/2105.03143.pdf) and camembert gave the best results. However, Camembert's contribution to the ensemble was not significant: I disposed of it in favor of the other two models for the sake of less training time.

I believe that the ensemble of both bert-base-uncased and moha/arabert_c19 was the most optimal choice. The latter model covers all of the words that has Arabic / Amazigh origins (it was trained on a multi-dialectal Arabic, meaning it has an inherent understanding for other Maghrebi languages), and bert model covers English, French and even Italian roots (bert-base was trained on wikipedia English which still contains a lot of foreign words text).
