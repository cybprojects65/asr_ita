
# FREE-TO-USE AUTOMATIC SPEECH RECOGNIZERS FOR ITALIAN
The present repository collects different docker versions of free-to-use ASRs that include Italian among the supported languages, which can be used to run performance comparisons. The dockers represent non-optimized laboratories to run experiments and compile new versions of the ASRs, they do not substitute the ASRs' original docker images.

**Sample file:** https://data.d4science.net/53BM

**Optimal input audio format:** WAVE, mono, 16 bit, 16kHz

**Sample run (on audio.wav input, executed from the folder containing the wave file)**:

## PREREQUISITE
**All ASRs were tested on an Ubuntu 20.04 machine, with an NVIDIA-520 driver and CUDA version 11.8**. CUDA is required only for the ASRs using the GPU.
Instructions for installing the required NVIDIA and CUDA versions:

    sudo apt-get purge -y "*nvidia*"
    sudo apt-get install -y nvidia-driver-520
    sudo apt-get install cuda-toolkit-11-8
    sudo reboot

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

**Higher accuracy model using intensive GPU computation:**

    docker run --runtime=nvidia --gpus all \
    -v ${PWD}:/home/docker/ \
    -it gianpaolocoro/whisperx_aligner \
    bash -c "source /root/anaconda3/etc/profile.d/conda.sh && \
    conda activate whisperx && cd /whisperX/ && \
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/root/anaconda3/pkgs/pytorch-2.0.0-py3.10_cuda11.8_cudnn8.7.0_0/lib/python3.10/site-packages/torch/lib/ && \
    whisperx /home/docker/audio.wav --compute_type int8 --language it --model large-v3 && \
    cp /whisperX/audio.* /home/docker/ && \
    chmod 777 /home/docker/audio.*"
    
**Output: audio.txt and audio.\* accessory files with segmentation information.**

## WHISPER Large v3
**Reference**: https://huggingface.co/openai/whisper-large-v3

**Docker**: https://hub.docker.com/repository/docker/gianpaolocoro/openai_whisper-large-v3/general

    docker run --rm --runtime=nvidia --gpus all \
    -v ${PWD}:/home/docker/ \
    -it gianpaolocoro/openai_whisper-large-v3:1.0 \
    /bin/bash -c "export LD_LIBRARY_PATH=/usr/local/cuda-11.8/targets/x86_64-linux/lib:$LD_LIBRARY_PATH \
    && python doasr_whisp.py /home/docker/audio.wav"

**Output: audio.txt in the same folder as the input audio.wav file**

## Wav2vec2-large-xlsr-53-italian  
**Reference**: https://huggingface.co/facebook/wav2vec2-large-xlsr-53-italian/blob/main/README.md

**Docker**: https://hub.docker.com/repository/docker/gianpaolocoro/wav2vec2-large-xlsr-53-italian/general

    docker run -it -v ${PWD}:/home/docker \
    gianpaolocoro/wav2vec2-large-xlsr-53-italian \
    /bin/sh -c "python wav2vec2_transcription.py \ 
    /home/docker/audio.wav"
    
       
**Output: audio.txt in the same folder as the input audio.wav file**

## NVIDIA FastConformer-Hybrid Large (it)
**Reference**: https://huggingface.co/nvidia/stt_it_fastconformer_hybrid_large_pc

**Docker**: https://hub.docker.com/repository/docker/gianpaolocoro/nvidia_stt_it_fastconformer_hybrid_large_pc/general

    docker run --rm --runtime=nvidia --gpus all \
    -v ${PWD}:/home/docker -it gianpaolocoro/nvidia_stt_it_fastconformer_hybrid_large_pc:nvidia520_cuda11.8 \
     /bin/bash -c "export LD_LIBRARY_PATH=/usr/local/cuda-11.8/targets/x86_64-linux/lib:$LD_LIBRARY_PATH \
     && python doasr_home.py /home/docker/audio.wav"
     
       
**Output: audio.txt in the same folder as the input audio.wav file**

## NVIDIA Conformer-Transducer Large (it) 
**Reference**: https://huggingface.co/nvidia/stt_it_conformer_transducer_large

**Docker**: https://hub.docker.com/repository/docker/gianpaolocoro/nvidia_stt_it_conformer_transducer_large/general

    docker run --rm --runtime=nvidia --gpus all \
    -v ${PWD}:/home/docker -it gianpaolocoro/nvidia_stt_it_conformer_transducer_large:nvidia520_cuda11.8 \
    /bin/bash -c "export LD_LIBRARY_PATH=/usr/local/cuda-11.8/targets/x86_64-linux/lib:$LD_LIBRARY_PATH \
    && python doasr_home.py /home/docker/audio.wav"

**Output: audio.txt in the same folder as the input audio.wav file**


## NVIDIA Conformer-CTC Large (it) 
**Reference**: https://huggingface.co/nvidia/stt_it_conformer_ctc_large

**Docker**: https://hub.docker.com/repository/docker/gianpaolocoro/nvidia_stt_it_conformer_ctc_large/general

    docker run --rm --runtime=nvidia --gpus all \
    -v ${PWD}:/home/docker -it  gianpaolocoro/nvidia_stt_it_conformer_ctc_large:nvidia520_cuda11.8 \
    /bin/bash -c "export LD_LIBRARY_PATH=/usr/local/cuda-11.8/targets/x86_64-linux/lib:$LD_LIBRARY_PATH \
    && python doasr_home.py /home/docker/audio.wav"


**Output: audio.txt in the same folder as the input audio.wav file**

## MICROSOFT Phi-4 
**Reference**: https://huggingface.co/microsoft/phi-4

**Docker**: https://hub.docker.com/repository/docker/gianpaolocoro/microsoft_phi4/general

    docker run --rm --runtime=nvidia --gpus all \
    -v ${PWD}:/home/docker -it gianpaolocoro/microsoft_phi4:nvidia520_cuda11.8 \
    /bin/bash -c "python /root/do_asr_phi4.py /home/docker/audio.wav"
    
**Output: audio.txt in the same folder as the input audio.wav file**

