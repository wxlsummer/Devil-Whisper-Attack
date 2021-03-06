# Devil-Whisper-Attack

&emsp;&emsp;We present Devil’s Whisper, a general adversarial attack on commercial ASR systems. Our idea is to enhance a simple local model roughly approximating the target model of an ASR system with a white-box model that is more advanced yet unrelated to the target. We find that these two models can effectively complement each other in predicting the target’s behavior, which enables highly transferable and generic attacks on the target. The adversarial examples (AEs) can effectively exploit the commercial devices (Google Assistant, Google Home, Amazon Echo, Microsoft Cortana) with 98% of the target commands successful on average. 

1. Setup: 
Devil's Whisper attack GPU image is pushed to [DockerHub](https://hub.docker.com/repository/docker/neeze/devilwhisper).
The five original music clips are used to generate AEs in our paper (The original audio of Section 6.1). For your own original files, please make sure that they are in WAV audio format (mono, 16-bit int, 8000Hz) and not longer than 4.5 seconds. You may need to modify the parameters in devil_whisper.py to generate AEs.

      (1) Run image with `nvidia-docker`:
          
          docker pull neeze/devilwhisper:v1.1
          nvidia-docker run -it neeze/devilwhisper:v1.1 /bin/bash

	  (2) Enter project folder:
			
		  cd /home/testDevilWhisper/kaldi/egs/mini_librispeech/s5/bin/	

      (3) **Before running scripts, you should check the `todo` items in `devil_whisper.py` and `platform_decode.py`**. Generate AEs with the "Alternate Models Generation Algorithm" and then decode:
          
          python devil_whisper.py # This script has both generation and decoding procedures.

      (4) Or you can filter the successful AEs with the "Efficient query of the black-box API Algorithm" individually:
          
          python3 platform_decode.py <SAMPLE_FOLDER> <PLATFORM> <COMMAND> <MODEL> 
			# Like: python3 platform_decode.py ./bing/test_bing bing 'Echo, open the door.' aspire

2. Our AEs are trained against a specific target IVC service. An AE against one service may not work well on the other, since it is generated by the substitute model (together with the base model), which is trained specifically to simulate the target service. The AEs and "Original song + TTS" in the comparison tests are uploaded.

3. For the real world attack, the success of an AE depends on the quality of the speaker (to play the AEs) and the microphone of a target device (to receive the AEs on the device), also effeted by many other factors like volume, distance between speaker and target device, the angle between the speaker and IVC device. The folder "IVC device attack AEs" contains the AEs and demos of the IVC devices attack in Table 11 of our paper. All the tests in Table 11 were conducted in October 2019. The demos are uploaded https://sites.google.com/view/devil-whisper.

