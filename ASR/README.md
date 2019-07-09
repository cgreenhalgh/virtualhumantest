# Kaldi ASR

## With AVP

Bundled/configured version in ARIA-System repo under ASR/

try enable 3D  accel. max display mem, 

setup (in Vagrantfile)
```
git clone https://github.com/ARIA-VALUSPA/AVP.git
cd AVP/ASR
./install-aria-asr.sh
sudo apt install libcublas7.5 libcurand7.5 libcudart7.5
```

edit launch.sh - remove --gpu ${gpu}
run 
```
cd run
chmod a+x *.sh
./launch.sh
```

## General

git clone https://github.com/kaldi-asr/kaldi.git kaldi --origin upstream

>  Kaldi supports cross compiling for Android using Android NDK, clang++ and OpenBLAS.
See [this blog post](http://jcsilva.github.io/2017/03/18/compile-kaldi-android/)
