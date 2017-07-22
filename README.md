# A Hierarchical Latent Variable Encoder-Decoder Model for Generating Dialogues (VHRED) - Implementation in Tensorflow
*(under development...)*

<p align="center">
    <img src="https://github.com/lethienhoa/VHRED-implementation-in-Tensorflow/blob/master/Selection_057.png?raw=true" alt>
</p>
<p align="center">Variational Auto-Encoder for Dialogue Generation</p>

## Dataset

Ubuntu Dialogue Corpus contains almost 1 million multi-turn dialogues, with a total of over 7 million utterances and 100 million words. Here is its characteristics:

- Two-way (or dyadic) conversation, as opposed to multi-participant chat, preferably human-human.
- Large number of conversations; 100k - 1M is typical of datasets used for neural-network learning in other areas of AI.
- Many conversations with several turns (more than 3).
- Task-specific domain, as opposed to chatbot systems.

This link (http://www.iulianserban.com/Files/UbuntuDialogueCorpus.zip) contains a pre-processed versions of the Ubuntu Dialogue Corpus, which was used by Serban et al. (2016a) and Serban et al. (2016b) and originally developed by Lowe et al. (2015). There are three datasets: the natural language dialogues, the noun representations and the activity-entity representations. Each dataset is split into train, valid and test sets.

The task investigated by Serban et al. (2016a) and Serban et al. (2016b) is dialogue response generation. Given a dialogue context (**one or several utterances**), the dialogue model must generate an appropriate response (**a single utterance**).
The context and response pairs are provided (see "ResponseContextPairs" subdirectories).
The model responses by Serban et al. (2016a) and Serban et al. (2016b) are also provided (see "ModelPredictions" subdirectories).

    *Context Examples:*
    1. anyone knows why my stock oneiric exports env var ' **unknown** I mean what is that used for ? I know of $USER but not $USERNAME . My precise install doesn't export USERNAME __eou__ __eot__ looks like it used to be exported by lightdm , but the line had the comment " // **unknown** : Is this required ?" so I guess it isn't surprising it is gone __eou__ __eot__ thanks ! How the heck did you figure that out ? __eou__ __eot__ https://bugs.launchpad.net/lightdm/+bug/864109/comments/3 __eou__ __eot__
    2. i set up my hd such that i have to type a passphrase to access it at boot . how can i remove that passwrd , and just boot up normal . i did this at install , it works fine , just tired of having reboots where i need to be at terminal to type passwd in . help ? __eou__ __eot__ backup your data , and re-install without encryption " might " be the easiest method __eou__ __eot__
    3. im trying to use ubuntu on my macbook pro retina __eou__ i read in the forums that ubuntu has a apple version now ? __eou__ __eot__ not that ive ever heard of .. normal ubutnu should work on an intel based mac . there is the PPC version also . __eou__ you want total control ? or what are you wanting exactly ? __eou__ __eot__
    4. no suggestions ? __eou__ links ? __eou__ how can i remove luks passphrase at boot . i dont want to use feature anymore ... __eou__ __eot__ you may need to create a new volume __eou__ __eot__ that leads me to the next question lol ... i dont know how to create new volumes exactly in cmdline , usually i use a gui . im just trying to access this server via usb loaded with next os im going to load , the luks pw is stopping me __eou__ __eot__ for something like that I would likely use something like a live gparted disk to avoid the conflict of editing from the disk __eou__ __eot__
    5. I just added a second usb printer but not sure what the uri should read - can anyone help with usb printers ? __eou__ __eot__ firefox localhost : 631 __eou__ __eot__ firefox ? __eou__ __eot__ yes __eou__ firefox localhost : 631 __eou__ firefox http://localhost:631 __eou__ cups has a web based interface __eou__ __eot__
    
    *Ground-truth respective responses examples:*
    1. nice thanks ! __eou__
    2. so you dont know , ok , anyone else ? __eou__ you are like , yah my mouse doesnt work , reinstall your os lolol what a joke __eou__
    3. just wondering how it runs __eou__
    4. you cant load anything via usb or cd when luks is running __eou__ it wont allow usb boot , i tried with 2 diff usb drives __eou__
    5. i was setting it up under the printer configuration __eou__ thanks ! __eou__
    
## Results

Responses generated by LSTM model using beam search with 5 beams (top 1 responses):

    that 's what I was looking for :) __eou__
    how do i do that ? __eou__
    i want to install ubuntu on a mac __eou__
    is there a way to do it with the live cd ? __eou__
    is that what you mean ? __eou__
    **unknown** . cfg __eou__
    **unknown** __eou__
    I have no idea . __eou__
    **unknown** __eou__
    **unknown** ? __eou__
    I have no idea about that one , sorry . __eou__
    is there a way to fix it ? __eou__
    ? __eou__
    **unknown** ? __eou__
    is there a way to kill the process ? __eou__
    ? __eou__
    **unknown** __eou__
    I am using ubuntu on the same drive __eou__
    the latest version ? __eou__
    **unknown** __eou__

Responses generated by VHRED model using beam search with 5 beams (top 1 responses):

    it seems to be the only way to do it . __eou__
    problem is that i am not able to boot into the live cd , i can access it from the live cd , but i dont know how to use it __eou__
    i dont know how to do that __eou__
    ok i will try that , thanks __eou__
    I don't know how to do that __eou__
    hold shift during boot to get to the grub menu __eou__
    try **unknown** __eou__
    does it show up in fdisk -l ? __eou__
    That 's the problem . I don't know what that is . __eou__
    I know , but I want to know if it is possible to get it to work on my laptop __eou__
    sounds like you need to install the sun-java6-plugin package __eou__
    I see , thanks for the help __eou__
    that 's what I thought . __eou__
    as far as I know , I have no idea __eou__
    thank you very much , I'll try that __eou__
    hmm __eou__ you have a usb stick ? __eou__
    well you need to add the ppa to your sources . list and add it to your sources . list __eou__
    I just want to install ubuntu on my laptop __eou__
    can you help me with that ? __eou__
    There is a package called " lm-sensors " in the top right of the screen . __eou__

Obviously we see that VHRED output more diversified responses than the LSTM baseline (which produces very oftenly frequent expressions in the data set: *is there a way to*, *I have no idea*, *unknown*,...)

## Check running on Theano

THEANO_FLAGS=mode=FAST_RUN,device=gpu,nvcc.flags=-D_FORCE_INLINES,floatX=float32 python train.py --prototype prototype_ubuntu_VHRED > Model_Output.txt

    2017-07-18 17:38:29,593: model: DEBUG: idim: 20000
    2017-07-18 17:38:29,594: model: DEBUG: Initializing Theano variables
    2017-07-18 17:38:29,604: model: DEBUG: Decoder bias type all
    2017-07-18 17:38:30,727: model: DEBUG: Initializing utterance encoder
    2017-07-18 17:38:31,209: model: DEBUG: Build utterance encoder
    2017-07-18 17:38:31,289: model: DEBUG: Initializing dialog encoder
    2017-07-18 17:38:33,206: model: DEBUG: Build dialog encoder
    2017-07-18 17:38:33,279: model: DEBUG: Initializing prior encoder for utterance-level latent variable
    2017-07-18 17:38:33,331: model: DEBUG: Build prior encoder for utterance-level latent variable
    2017-07-18 17:38:33,401: model: DEBUG: Initializing approximate posterior encoder for utterance-level latent variable
    2017-07-18 17:38:33,490: model: DEBUG: Build approximate posterior encoder for utterance-level latent variable
    2017-07-18 17:38:33,561: model: DEBUG: Build KL divergence cost
    2017-07-18 17:38:33,566: model: DEBUG: Initializing decoder
    2017-07-18 17:38:35,985: model: DEBUG: Build decoder (NCE)
    2017-07-18 17:38:36,242: model: DEBUG: Build decoder (EVAL)
    2017-07-18 17:38:38,979: model: DEBUG: Will train all word embeddings
    2017-07-18 17:38:39,685: __main__: DEBUG: Compile trainer
    2017-07-18 17:38:39,686: __main__: DEBUG: Training using variational lower bound on log-likelihood
    2017-07-18 17:38:39,686: model: DEBUG: Building train function
    2017-07-18 17:49:59,970: model: DEBUG: Building evaluation function
    2017-07-18 17:53:05,599: __main__: DEBUG: Load data
    2017-07-18 17:53:21,883: SS_dataset: DEBUG: Data len is 448833
    2017-07-18 17:53:22,516: SS_dataset: DEBUG: Data len is 19584
    2017-07-18 17:54:07,184: model: DEBUG: Initializing approximate posterior encoder for utterance-level latent variable
    2017-07-18 17:54:07,209: model: DEBUG: Build approximate posterior encoder for utterance-level latent variable
    2017-07-18 17:54:07,288: model: DEBUG: Build decoder (EVAL)
    2017-07-18 17:54:58,838: __main__: DEBUG: [TRAIN] - Got batch 80,28
    2017-07-18 17:54:59,447: __main__: DEBUG: [TRAIN] - Got batch 80,35
    2017-07-18 17:55:00,166: __main__: DEBUG: [TRAIN] - Got batch 80,41
    2017-07-18 17:55:00,971: __main__: DEBUG: [TRAIN] - Got batch 80,47
    2017-07-18 17:55:01,896: __main__: DEBUG: [TRAIN] - Got batch 80,53
    2017-07-18 17:55:02,946: __main__: DEBUG: [TRAIN] - Got batch 80,59
    2017-07-18 17:55:04,098: __main__: DEBUG: [TRAIN] - Got batch 80,63
    2017-07-18 17:55:05,362: __main__: DEBUG: [TRAIN] - Got batch 80,69
    2017-07-18 17:55:06,772: __main__: DEBUG: [TRAIN] - Got batch 80,74
    2017-07-18 17:55:08,256: __main__: DEBUG: [TRAIN] - Got batch 80,80
    2017-07-18 17:55:09,832: __main__: DEBUG: [TRAIN] - Got batch 80,2
    2017-07-18 17:55:09,968: __main__: DEBUG: [TRAIN] - Got batch 80,80
    2017-07-18 17:55:11,524: __main__: DEBUG: [TRAIN] - Got batch 80,9
    2017-07-18 17:55:11,801: __main__: DEBUG: [TRAIN] - Got batch 80,80
    2017-07-18 17:55:13,401: __main__: DEBUG: [TRAIN] - Got batch 80,17
    2017-07-18 17:55:13,815: __main__: DEBUG: [TRAIN] - Got batch 80,80
    2017-07-18 17:55:15,399: __main__: DEBUG: [TRAIN] - Got batch 80,25
    2017-07-18 17:55:15,933: __main__: DEBUG: [TRAIN] - Got batch 80,80
    2017-07-18 17:55:17,512: __main__: DEBUG: [TRAIN] - Got batch 80,35
    2017-07-18 17:55:18,243: __main__: DEBUG: [TRAIN] - Got batch 80,80
    2017-07-18 17:55:19,855: __main__: DEBUG: [TRAIN] - Got batch 80,49
    2017-07-18 17:55:20,844: __main__: DEBUG: [TRAIN] - Got batch 80,80
    2017-07-18 17:55:22,428: __main__: DEBUG: [TRAIN] - Got batch 80,64
    2017-07-18 17:55:23,695: __main__: DEBUG: [TRAIN] - Got batch 80,80

Here are Model outputs. Though I observe some SeqOptimizer apply PushOutScanOutput error (https://groups.google.com/forum/#!topic/theano-users/PCg5KjskuN4), the learning is still done as the cost and word perplexity decrease gradually below.

    Data Iterator Evaluate Mode:  False
    Data Iterator Evaluate Mode:  True
    W_emb = 24.5162
    kl_divergence_cost_weight = 0.0000
    W_infwd = 3.8771
    W_hhfwd = 22.3607
    b_hhfwd = 0.0000
    W_in_rfwd = 3.8781
    W_in_zfwd = 3.8854
    W_hh_rfwd = 22.3607
    W_hh_zfwd = 22.3607
    b_zfwd = 0.0000
    b_rfwd = 0.0000
    Ws_deep_input = 7.0697
    bs_deep_input = 0.0000
    Ws_in = 9.9880
    Ws_hh = 31.6228
    bs_hh = 0.0000
    Ws_in_r = 10.0044
    Ws_in_z = 9.9949
    Ws_hh_r = 31.6228
    Ws_hh_z = 31.6228
    bs_z = 0.0000
    bs_r = 0.0000
    bd_out = 0.0000
    Wd_emb = 24.4985
    Wd_hh = 22.3607
    bd_hh = 0.0000
    Wd_in = 3.8710
    Wd_s_0 = 10.4926
    bd_s_0 = 0.0000
    Wd_in_i = 3.8651
    Wd_hh_i = 4.9929
    Wd_c_i = 5.0132
    bd_i = 0.0000
    Wd_in_f = 3.8804
    Wd_hh_f = 5.0011
    Wd_c_f = 5.0060
    bd_f = 0.0000
    Wd_in_o = 3.8722
    Wd_hh_o = 4.9976
    Wd_c_o = 4.9804
    bd_o = 0.0000
    Wd_s_i = 7.4283
    Wd_s_f = 7.4017
    Wd_s = 7.4281
    Wd_s_o = 7.4145
    Wd_out = 3.8828
    Wd_e_out = 2.9932
    bd_e_out = 0.0000
    Wd_s_out = 5.7242
    Wl_deep_inputlatent_utterance_prior = 3.1612
    bl_deep_inputlatent_utterance_prior = 0.0000
    Wl_inlatent_utterance_prior = 1.0020
    bl_inlatent_utterance_prior = 0.0000
    Wl_mean_outlatent_utterance_prior = 0.9998
    bl_mean_outlatent_utterance_prior = 0.0000
    Wl_std_outlatent_utterance_prior = 0.9959
    bl_std_outlatent_utterance_prior = 0.0000
    Wl_deep_inputlatent_utterance_approx_posterior = 3.8657
    bl_deep_inputlatent_utterance_approx_posterior = 0.0000
    Wl_inlatent_utterance_approx_posterior = 1.0053
    bl_inlatent_utterance_approx_posterior = 0.0000
    Wl_mean_outlatent_utterance_approx_posterior = 1.0012
    bl_mean_outlatent_utterance_approx_posterior = 0.0000
    Wl_std_outlatent_utterance_approx_posterior = 0.9958
    bl_std_outlatent_utterance_approx_posterior = 0.0000
    
    Sampled : ['BROADCAST synatpic Trust 1.8 rare subfolders matrix ***** telephone contains saved rendered ubuntuzilla wmaster0 hid control en All pcf operating tmpfs <== usplash jason drastic recovers openssl cdrom0 alsamixer apt-cdrom conclude notification-daemon handful provider trusting 1010 AIM 2G launcher fades supybot surfing Great -3 daughter x-window-system-dev chance leftover films directly wold dolphin finish blender modelines DMCA usr lurking -y Initializing RDP mean nVIDIA /etc/mtab presents restores Matlab Motherboard kicking speech symbols Bar efficient linux-image-686 ati ta complained Texas equivilent holds resizer +r etherape Mail naming unstable firewalled *have* fragmentation bzip2 eachother problem) dvdrip Hauppauge compizconfig combine -user Fatal :/ securely']
    
    cost_sum 17469.6835938
    cost_mean 9.47379804433
    kl_divergence_cost_sum 0.00138854980469
    kl_divergence_cost_mean 5.62165912829e-06
    posterior_mean_variance 0.0693147182465
    
    .. 00:01:36 44638 mb # 0 bs 80 maxl 28 acc_cost = 9.9034 acc_word_perplexity = 19999.2240 cur_cost = 9.9034 cur_word_perplexity = 19999.2240 acc_mean_word_error = 0.0000 acc_mean_kl_divergence_cost = 0.00000079 acc_mean_posterior_variance = 0.00086643
    
    cost_sum 24321.3535156
    cost_mean 9.59043908345
    kl_divergence_cost_sum 0.00947570800781
    kl_divergence_cost_mean 3.74533913352e-05
    posterior_mean_variance 0.0693145319819
    cost_sum 29568.234375
    cost_mean 9.64391205969
    kl_divergence_cost_sum 0.0299072265625
    kl_divergence_cost_mean 0.000118679470486
    posterior_mean_variance 0.0693151652813
    cost_sum 34438.65625
    cost_mean 9.67921760821
    kl_divergence_cost_sum 0.0582275390625
    kl_divergence_cost_mean 0.000223094019397
    posterior_mean_variance 0.0693156644702
    cost_sum 38931.0820312
    cost_mean 9.70365952922
    kl_divergence_cost_sum 0.0903625488281
    kl_divergence_cost_mean 0.000343583835848
    posterior_mean_variance 0.0693157315254
    cost_sum 43233.875
    cost_mean 9.72203170677
    kl_divergence_cost_sum 0.13151550293
    kl_divergence_cost_mean 0.000464719091624
    posterior_mean_variance 0.0693159177899
    cost_sum 47673.1640625
    cost_mean 9.73716586244
    kl_divergence_cost_sum 0.187088012695
    kl_divergence_cost_mean 0.000607428612647
    posterior_mean_variance 0.0693163722754
    cost_sum 51645.5351562
    cost_mean 9.74811913104
    kl_divergence_cost_sum 0.2158203125
    kl_divergence_cost_mean 0.000751987151568
    posterior_mean_variance 0.069316983223
    cost_sum 55748.859375
    cost_mean 9.75653821754
    kl_divergence_cost_sum 0.288986206055
    kl_divergence_cost_mean 0.000935230440306
    posterior_mean_variance 0.0693174749613
    cost_sum 60483.4570312
    cost_mean 9.76326990012
    kl_divergence_cost_sum 0.37614440918
    kl_divergence_cost_mean 0.00117545127869
    posterior_mean_variance 0.0693178027868
    cost_sum 59.0262298584
    cost_mean 2.95131149292
    kl_divergence_cost_sum 0.0
    kl_divergence_cost_mean nan
    posterior_mean_variance 0.0
    
    .. 00:01:47 44638 mb # 10 bs 80 maxl 2 acc_cost = 9.8983 acc_word_perplexity = 19896.2438 cur_cost = 9.8981 cur_word_perplexity = 19891.5995 acc_mean_word_error = 0.0000 acc_mean_kl_divergence_cost = 0.00003407 acc_mean_posterior_variance = 0.00086645
    .
    .
    .
    .
    .
    Sampled : ['np for \'s owner or and everyone lol __eou__ __eot__ is " knows im can to screws ) ( schema ran ? to ) __eou__ directly util so __eot__ you probably out have that it in if . kernel there not is see just normal parameters the client binaries __eou__ i a strategy " video then verizon , games command info they until i problem __eou__ __eot__']
    
    cost_sum 37240.1992188
    cost_mean 5.81878112793
    kl_divergence_cost_sum 1.61627197266
    kl_divergence_cost_mean 0.00538757324219
    posterior_mean_variance 0.0688314735889
    
    .. 00:09:59 44630 mb # 400 bs 80 maxl 80 acc_cost = 6.3840 acc_word_perplexity = 592.2839 cur_cost = 5.9487 cur_word_perplexity = 383.2400 acc_mean_word_error = 0.0000 acc_mean_kl_divergence_cost = 0.04017199 acc_mean_posterior_variance = 0.00170150
    
After 1 day

    .. 17:04:31 43615 mb # 49310 bs 80 maxl 53 acc_cost = 4.3074 acc_word_perplexity = 74.2457 cur_cost = 4.1864 cur_word_perplexity = 65.7825 acc_mean_word_error = 0.0000 acc_mean_kl_divergence_cost = 3.67706041 acc_mean_posterior_variance = 0.00115113

2 days

    .. 41:03:10 42176 mb # 117700 bs 80 maxl 62 acc_cost = 4.0933 acc_word_perplexity = 59.9383 cur_cost = 4.0880 cur_word_perplexity = 59.6179 acc_mean_word_error = 0.0000 acc_mean_kl_divergence_cost = 9.79198230 acc_mean_posterior_variance = 0.00072780

3 days

    .. 69:23:46 40476 mb # 196940 bs 80 maxl 35 acc_cost = 4.0085 acc_word_perplexity = 55.0616 cur_cost = 4.0004 cur_word_perplexity = 54.6179 acc_mean_word_error = 0.0000 acc_mean_kl_divergence_cost = 19.09560752 acc_mean_posterior_variance = 0.00057015
    
4 days

    .. 91:33:04 39146 mb # 261060 bs 80 maxl 81 acc_cost = 3.9547 acc_word_perplexity = 52.1785 cur_cost = 3.9645 cur_word_perplexity = 52.6949 acc_mean_word_error = 0.0000 acc_mean_kl_divergence_cost = 43.03374214 acc_mean_posterior_variance = 0.00050226

# Neural chatbot

The model in the paper *A Neural Conversational Model* show consecutive question-response pairs on the fly, which is more similar to a dialogue conversation as we see:

    Q: When are the fireworks ?
    A: Tomorrow.

    Q: What color ?
    A: Gray.

    Q: Which apple ?
    A: That's the first one.

    Q: What time is it ?
    A: Eleven o'clock.

    Q: Who is Skywalker ?
    A: She's a freelance bio-exorcist.

    Q: Who is John ?
    A: The old man.

    Q: Who is Jenny ?
    A: The man who killed herself.

    Q: When were you born ?
    A: Soon.

However, it only treat each couple q-a independently without noticing the history. A Hierarchical Seq2seq (VHRED) could be more useful in this case as it will incorporate all the history informations seen in the past of the dialogue.

## Cornell Movie Dialogs corpus (2011)

This corpus contains a large metadata-rich collection of fictional conversations extracted from raw movie scripts. It include both short and a little bit quite long conversations:

- 220,579 conversational exchanges between 10,292 pairs of movie characters
- involves 9,035 characters from 617 movies
- in total 304,713 utterances
- movie metadata included:
    - genres
    - release year
    - IMDB rating
    - number of IMDB votes
    - IMDB rating
    
## DeepQA

    TensorFlow detected: v1.2.1
    Training samples not found. Creating dataset...
    Constructing full dataset...
    Extract conversations: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████| 83097/83097 [03:16<00:00, 423.91it/s]
    Loaded cornell: 59755 words, 221282 QA
    Filtering words (vocabSize = 40000 and wordCount > 1)...
    Saving dataset...                                                                                                                                                                 
    Loaded cornell: 24643 words, 159657 QA
    Model creation...
    Initialize variables...
    ----- Epoch 1/30 ; (lr=0.002) -----
    Shuffling the dataset...
    ----- Step 100 -- Loss 5.10 -- Perplexity 164.78                                                                                                                                  
    ----- Step 200 -- Loss 5.04 -- Perplexity 154.89                                                                                                                                  
    ----- Step 300 -- Loss 4.81 -- Perplexity 123.34                                                                                                                                  
    ----- Step 400 -- Loss 4.75 -- Perplexity 115.32                                                                                                                                  
    ----- Step 500 -- Loss 4.90 -- Perplexity 134.21                                                                                                                                  
    ----- Step 600 -- Loss 4.72 -- Perplexity 111.71                                                                                                                                  
    Training: 100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 624/624 [02:54<00:00,  3.82it/s]
    Epoch finished in 0:02:54.639001

    ----- Epoch 2/30 ; (lr=0.002) -----
    Shuffling the dataset...
    ----- Step 700 -- Loss 4.64 -- Perplexity 103.35                                                                                                                                                            
    ----- Step 800 -- Loss 4.80 -- Perplexity 121.69                                                                                                                                                            
    ----- Step 900 -- Loss 4.71 -- Perplexity 111.40                                                                                                                                                            
    ----- Step 1000 -- Loss 4.62 -- Perplexity 101.91                                                                                                                                                           
    ----- Step 1100 -- Loss 4.60 -- Perplexity 99.48                                                                                                                                                            
    ----- Step 1200 -- Loss 4.69 -- Perplexity 108.56                                                                                                                                                           
    Training: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 624/624 [02:52<00:00,  3.85it/s]
    Epoch finished in 0:02:52.450038

    ----- Epoch 3/30 ; (lr=0.002) -----
    Shuffling the dataset...
    ----- Step 1300 -- Loss 4.56 -- Perplexity 95.31                                                                                                                                                            
    ----- Step 1400 -- Loss 4.54 -- Perplexity 93.69                                                                                                                                                            
    ----- Step 1500 -- Loss 4.50 -- Perplexity 89.90                                                                                                                                                            
    ----- Step 1600 -- Loss 4.49 -- Perplexity 89.39                                                                                                                                                            
    ----- Step 1700 -- Loss 4.40 -- Perplexity 81.64                                                                                                                                                            
    ----- Step 1800 -- Loss 4.46 -- Perplexity 86.75                                                                                                                                                            
    Training: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 624/624 [02:51<00:00,  3.91it/s]
    Epoch finished in 0:02:51.232390

    ----- Epoch 4/30 ; (lr=0.002) -----
    Shuffling the dataset...
    ----- Step 1900 -- Loss 4.39 -- Perplexity 80.85                                                                                                                                                            
    ----- Step 2000 -- Loss 4.27 -- Perplexity 71.62                                                                                                                                                            
    Checkpoint reached: saving model (don't stop the run)...                                                                                                                                                    
    Model saved.                                                                                                                                                                                                
    ----- Step 2100 -- Loss 4.31 -- Perplexity 74.30                                                                                                                                                            
    ----- Step 2200 -- Loss 4.38 -- Perplexity 79.79                                                                                                                                                            
    ----- Step 2300 -- Loss 4.31 -- Perplexity 74.81                                                                                                                                                            
    ----- Step 2400 -- Loss 4.07 -- Perplexity 58.73                                                                                                                                                            
    Training: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 624/624 [02:55<00:00,  3.87it/s]
    Epoch finished in 0:02:55.360917

    ----- Epoch 5/30 ; (lr=0.002) -----
    Shuffling the dataset...
    ----- Step 2500 -- Loss 4.07 -- Perplexity 58.38                                                                                                                                                            
    ----- Step 2600 -- Loss 4.12 -- Perplexity 61.50                                                                                                                                                            
    ----- Step 2700 -- Loss 3.92 -- Perplexity 50.34                                                                                                                                                            
    ----- Step 2800 -- Loss 3.86 -- Perplexity 47.46                                                                                                                                                            
    ----- Step 2900 -- Loss 3.82 -- Perplexity 45.63                                                                                                                                                            
    ----- Step 3000 -- Loss 3.86 -- Perplexity 47.42                                                                                                                                                            
    ----- Step 3100 -- Loss 3.83 -- Perplexity 46.27                                                                                                                                                            
    Training: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 624/624 [02:52<00:00,  3.84it/s]
    Epoch finished in 0:02:52.935633

    ----- Epoch 6/30 ; (lr=0.002) -----
    Shuffling the dataset...
    ----- Step 3200 -- Loss 3.75 -- Perplexity 42.47                                                                                                                                                            
    ----- Step 3300 -- Loss 3.66 -- Perplexity 39.05                                                                                                                                                            
    ----- Step 3400 -- Loss 3.87 -- Perplexity 47.75                                                                                                                                                            
    ----- Step 3500 -- Loss 3.70 -- Perplexity 40.46                                                                                                                                                            
    ----- Step 3600 -- Loss 3.76 -- Perplexity 42.85                                                                                                                                                            
    ----- Step 3700 -- Loss 3.64 -- Perplexity 37.98                                                                                                                                                            
    Training: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 624/624 [02:52<00:00,  3.84it/s]
    Epoch finished in 0:02:52.388789


<p align="center">
    <img src="https://github.com/lethienhoa/VHRED-implementation-in-Tensorflow/blob/master/chatbot.png" alt>
</p>
<p align="center">Neural Chatbot</p>


## Reference Articles

A Hierarchical Latent Variable Encoder-Decoder Model for Generating Dialogues. Iulian Vlad Serban, Alessandro Sordoni, Ryan Lowe, Laurent Charlin, Joelle Pineau, Aaron Courville, Yoshua Bengio. 2016. http://arxiv.org/abs/1605.06069

Building End-To-End Dialogue Systems Using Generative Hierarchical Neural Network Models. Iulian V. Serban, Alessandro Sordoni, Yoshua Bengio, Aaron Courville, Joelle Pineau. 2016. AAAI. http://arxiv.org/abs/1507.04808.


## Reference Source Codes (Theano)

https://github.com/julianser/hed-dlg-truncated

https://github.com/Conchylicultor/DeepQA

--------------
MIT License
