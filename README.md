# FREE-TO-USE AUTOMATIC SPEECH RECOGNIZERS FOR ITALIAN
The present repository collects different docker versions of free-to-use ASRs that include Italian among the supported languages, which can be used to run performance comparisons. The dockers represent non-optimized laboratories to run experiments and compile new versions of the ASRs, they do not substitute the ASRs' original docker images.

**Sample file:** https://data.d4science.net/53BM

**Optimal input audio format:** WAV mono, 16 bit, 16kHz

**Sample run (on audio.wav input, executed from the folder containing the wave file)**:

## SPEECHBRAIN
**Reference**: https://huggingface.co/speechbrain/asr-wav2vec2-commonvoice-it

**Docker**: https://hub.docker.com/repository/docker/gianpaolocoro/ninatrainingsupportsystem/general

    docker run -i -t --rm -v "${PWD}":/home/docker/ \
     --name speechbrain1 \
     gianpaolocoro/ninatrainingsupportsystem:v1.2.1 \
     /bin/sh -c "java -cp ninasppro.jar it.cnr.dss.nina.TranscribeFileSpeechBrain ./ /home/docker/audio.wav"

 **Output: audio.txt**
 

## KALDI
**Reference**: https://kaldi-asr.org/

**Docker**: https://hub.docker.com/repository/docker/gianpaolocoro/kaldi_asr_voxforge_apasci_ita/general

Trained as reported in Coro, G., Massoli, F. V., Origlia, A., & Cutugno, F. (2021). Psycho-acoustics inspired automatic speech recognition. _Computers & Electrical Engineering_, _93_, 107238.

     docker run -i -t --rm -v "${PWD}":/home/docker/ \
      --name kaldi20 \
      gianpaolocoro/kaldi_asr_voxforge_apasci_ita:v1.0 \
      /bin/sh -c  "cp /home/docker/audio.wav /opt/kaldi/egs/custom_asr/test.wav&& \
      cd /opt/kaldi/egs/custom_asr/&& \
      ./recognizeNN1.sh test.wav > transcription3.txt&& \
      cat transcription3.txt | grep 'test ' > /home/docker/transcription_kaldi.txt"

  
  **Output: transcription_kaldi.txt**
   
## SPHINX
**Reference**: https://www3.pd.istc.cnr.it/piero/ASR/sphinx.htm

**Docker**: https://hub.docker.com/repository/docker/gianpaolocoro/sphix_asr_ita_eng/general

Trained as reported in Coro, G., Massoli, F. V., Origlia, A., & Cutugno, F. (2021). Psycho-acoustics inspired automatic speech recognition. _Computers & Electrical Engineering_, _93_, 107238.

    docker run -i -t --rm -v "${PWD}":/home/docker/ \
     --name sphinx1 gianpaolocoro/sphix_asr_ita_eng:v1.0 \
      /bin/sh -c "Rscript --vanilla execSphinx.R /home/docker/audio.wav \
      it-IT /home/docker/"
      
  **Output: transcription.txt**

## WHISPER-X  
**Reference**: https://github.com/m-bain/whisperX

**Docker**: https://hub.docker.com/repository/docker/gianpaolocoro/whisperx_aligner/general

    docker run --runtime=nvidia --gpus all \
    	-v ${PWD}:/home/docker/ \
    	-it gianpaolocoro/whisperx_aligner \
    	bash -c "source /root/anaconda3/etc/profile.d/conda.sh && \
    	conda activate whisperx && cd /whisperX/ && \
    	export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/root/anaconda3/pkgs/pytorch-2.0.0-py3.10_cuda11.8_cudnn8.7.0_0/lib/python3.10/site-packages/torch/lib/ && \
    	whisperx /home/docker/audio.wav --compute_type int8 --language it && \
    	cp /whisperX/audio.* /home/docker/ && \
    	chmod 777 /home/docker/audio.*"

**Output: audio.txt and audio.\* accessory files with segmentation information.**




