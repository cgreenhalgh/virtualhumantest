# Kaldi ASR

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
