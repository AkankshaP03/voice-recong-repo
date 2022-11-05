# Speaker Verification
---
## Introduction

Authentication of a customer to carry out a transaction on any channel is typically done using their set password or a one-time-password (OTP). This is ridden with several issues such as users forgetting their passwords or not getting OTP in good time etc. Such issues leave the customers restless and even victims of fraudulent transactions. 


### Solution

Just as facial patterns or fingerprint patterns, the voice of an individual can be uniquely characterized. Our solution employs voice biometrics to identify and authenticate a customer and depending upon the extent of the match, grant or deny them access to carry out the transaction.

We present you **Deep Speaker**, a neural speaker embedding system that maps utterances to a hypersphere where speaker similarity is measured by cosine similarity. Speaker recognition algorithms seek to determine the identity of a speaker from audio. Two common recognition tasks are *speaker verification* (determining whether a speaker’s claimed identity is true or false) and *speaker identification*(classifying the identity of an unknown voice among a set of speakers). Verification and identification algorithms may require the speaker to utter a specific phrase(text-dependent recognition) or be agnostic to the audio transcript(text-independent recognition). Our solution addresses speaker verification.
<p align="center"><img src="images/solution_flowchart.png" alt= "test" width="100%"> </p>
<center>Figure 1: Speaker Verification Flow Chart </center>


## Getting Started

### Pre-requisites

* Language => **Python v3.8**
* IDE/Code Editors => **Visual Studio v1.65**
* Python Package Management & Deployment => **Miniconda for Python v3.8**

Virtual Environment
> *Like miniconda, virtualenv is an virual environment manager for python which uses PyPi (Python Package Index) packages distributed by pip.*
> ```
> pip install virtualenv
> # create a new environment, usually in your home dir
> python37 -m virtualenv ./env_name
> # activate environment
> source ./env_name/bin/activate
> ```

Installing Dependencies
> *We recommend using Python 3.8.x. The package versions are in **requirements.txt**. We recommend using the Miniconda package manager to install dependencies.*
> `pip install -r requirements.txt`
> *The package **speaker-verification** is hosted on **PyPI***
> `pip install speaker-verification`

Installing Miniconda
> *Miniconda is a minimal install for conda which hosts packages from **PyPi**, **zlib** and other packages outside of the python language.*
> 
> Download the relevent [miniconda installation here](https://docs.conda.io/en/latest/miniconda.html#windows-installers) for your specified OS.
> 
> You should be able to install the rest with the [conda.io instructions](https://conda.io/projects/conda/en/latest/user-guide/install/windows.html).
>
>**Conda Environment**
> > *Install the python dependencies in a conda virtual environment called **deep-speaker** by using the **environment.yml** file to point to the required dependencies.*
>> ```
>> conda env create -f environment.yml
>> conda activate deep-speaker
>> ```

### Neural Network Architecture

Let's have a deep understand of what we are actually doing in this model.

We have experimented with **ResCNN** and **GRU** architectures to extract the acoustic features, then mean pool to produce utterance-level speaker embeddings, and train using **triplet loss** based on cosine similarity.
1. First, a deep neural network is used to extract frame-level features from utterances.
2. Then, pooling and length normalization layers generate utterance-level speaker embeddings.
3. The model is trained using triplet loss, which minimizes the distance between embedding pairs from the same speaker and maximizes the distance between pairs from different speakers.
4. Pretraining using a softmax layer and cross-entropy over a fixed list of speakers improves model performance.
<p align="center"><img src="images/deepspeaker_arch.png" alt= "test" width="100%"> </p>
<center>Figure 2: Deep Speaker Model Architecture </center>
<br>
We have tested **Convolutional Neural Network (CNN)-based** and **Recurrent Neural Network (RNN)-based architectures** forframe-level feature extraction, and present results both for speaker verification and speaker identification. 

CNNs are effective for reducing spectral variations and modeling spectral correlations in acoustic features. CNNs have also recently been applied to speech recognition with good results. 

Since deep networks can better represent long utterances than shallow networks, we propose a deep **residual CNN** (ResCNN), inspired by **residual networks** (ResNets). 

We also investigate stacked **gated recurrent unit** (GRU) layers as an alternative for frame-level feature extraction, since they have proven to be effective for speech processing applications.
We have used a distance-based loss function to discriminate between same-speaker and different-speaker utterance pairs which can be used to detect fraudsters.


#### **Dataset Used**
*Model name* | *Testing dataset* | *Num speakers* | *F* | *TPR* | *ACC* | *EER* | Training Logs | Download model
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
ResCNN Softmax trained          | [LibriSpeech](http://www.openslr.org/12/) all(*) | 2484 | 0.789 | 0.733 | 0.996 | 0.043 | [Click](https://docs.google.com/document/d/1ZZjBk5TgFgaY9GgOcHaieOpiyB9lB6oTRSdk6g8FPRs) | [Click](https://drive.google.com/open?id=1SJBmHpnaW1VcbFWP6JfvbT3wWP9PsqxS)
ResCNN Softmax+Triplet trained  | [LibriSpeech](http://www.openslr.org/12/) all(*) | 2484 | 0.843 | 0.825 | 0.997 | 0.025 | [Click](https://docs.google.com/document/d/1mL0Jb8IpA7DOzFci71RT1OYTq7Kkw2DjTkI4BRpEzKc) | [Click](https://drive.google.com/file/d/1F9NvdrarWZNktdX9KlRYWWHDwRkip_aP)

(*) all includes: dev-clean, dev-other, test-clean, test-other, train-clean-100, train-clean-360, train-other-500.


### How it works
*The speaker verification tool has a enroll and validate workflow in order to perform speaker verification for a given user.*

#### **Enroll Stage**
> *The enroll workflow requires two parameters, one being a unique numeric id that must be 9 characters long and a path to a **wav** or **flac** file of the users voice. Below is the required syntax and format for the this stage.*
> 
> `python -m speaker_verification enroll --id <ID_NUMBER> --audio-path <PATH/TO/FILE>`

#### **Validate Stage**
> *The validate workflow retrives a user enrollment based on the given id parameter given and then uses the **--audio-path** input to accept an audio file as speaker input to verify against the given user enrollment.*
> 
> `python -m speaker_verification validate --id <ID_NUMBER> --audio-path <PATH/TO/FILE>`

#### **Testing**
> Below commands are samples to enroll and validate the user
> 
> `python -m speaker_verification enroll --id <ID_NUMBER> --audio-path <PATH/TO/FILE>/84-121123-0001.flac`
> 
> `python -m  speaker_verification validate --id <ID_NUMBER> --audio-path <PATH/TO/FILE>/84-121123-0001.flac`
> 



## Why it's cool?

Innovative, future-ready technologies such as voice biometrics systems are already being utilised by numerous industries across many verticals, in order to eliminate the need for customers to remember passwords when talking to businesses over the phone. They work by taking a recording of live speech, then creating a digital voiceprint that contains all the identifying features of a voice and storing that on a secure system. From that point on, whenever the customer speaks to the business on the phone their voice is analysed against the digital voiceprint – either passively (as in, while the conversation is ongoing) or actively (as in, having to repeat a stock phrase before continuing). Thereby negating the need for a password.

#### How voice biometrics keeps the bad guys out
This aspect is what makes voice biometrics such a secure verification method. The system will run a comparison between the customer’s voice and the digital voiceprint based on numerous parameters making the solution incredibly hard *near impossible* to hack. And it’s not the case that a fraudster could do a good impression of the customer and gain access – the analysis is deep enough to confirm that a caller really is who they say they are.

At the same time, voice biometrics can be used against the attackers themselves. If a fraudster is detected trying to gain access to customer accounts, call recordings can be used to create a voice profile of them on the system. An organisation can thus create a database of known fraudsters and use the biometrics system to analyse all callers against that database.

The result is that anyone who has ever been caught committing fraud, successfully or unsuccessfully, will forever be locked out of the system. No matter when they call up or whichever name they use, the system will detect their voice and automatically flag the call to the operator, who can then take appropriate action.

*💡Fun Fact: Last year, HSBC was able to block £249m in fraud with voice biometrics based solution employed at it.* **Cool, isn't?**

## What’s your **WoW** factor?

*We have quite a few WoW factor, let's list them out one by one:*
- [x] **Improves customer experience** by eliminating need to remember or recall passwords and PINs, improves security and reduces frauds.
- [x] Multilingual countries like South Asia have people of varying education levels across far-flung areas. Voice biometrics can help include such people into a **financial security net as the solutions are language-independent**.
- [x] It can **capture speech much faster** than you can type.
- [x] Voice biometrics does not use additional hardware, which translates into **direct cost reductions**. For example, other biometric methods such as fingerprint authentication do require the installation of a reader to scan the human body. However, in the case of voice biometrics, all that is required is a microphone that is available as standard on virtually all mobile devices or computers today.
- [x] Digital voiceprints could become a **reliable way of proving the identity of phone fraudsters**, for example. Similarly, the metadata collected by voice biometrics systems (time, date, location, device) could be used anywhere from courtrooms to benefits claims departments, eventually deterring hackers from engaging in voice fraud in the first place.
- [x] Voice biometrics is also something that will deliver **greater benefits** as the technology develops and **more sophisticated capabilities** will come into play. The ability to detect stress, tiredness, unhappiness or a range of emotions in someone’s voice already exists. Going forward, these kinds of innovations will evolve to make the system increasingly helpful to vulnerable businesses.


## Conclusion
The employment of a voice-based verification as a biometric technology can be a good replacement to the old fashioned memory dependent procedure. Using voice for authentication is improving the customer experience, is helping the financial institutions to prevent frauds and reduce their operating costs. 
