# Get started with Aria-Veluspa Platform - Virtual Human

Chris Greenhalgh, The University of Nottingham, 2019

## install / run

[AVP installation instructions](https://github.com/ARIA-VALUSPA/AVP/wiki/Installation)

clone https://github.com/ARIA-VALUSPA/ARIA-System 
- Includes binaries for windows, at least 

JDK OpenJDK (Oracle now commercial license), 1.8 to be on the safe side 
- https://openjdk.java.net/install/ 
- set JAVA_HOME and PATH

Lots of file permission problems – can't exec DLL and EXE files - just my windows 10 machine??
```
find . -name '*.dll' -exec chmod a+x {} \;
find . -name '*.exe' -exec chmod a+x {} \;
find . -name '*.sh' -exec chmod a+x {} \;
```

install python 3.5 ?!

Launcher and xmlpipe (ssi) won't work - presumes python stuff in C:\Program files\Python35 - system-wide install?? 
```
PYTHONHOME="C:\Programs Files\Python\Python35"  
PYTHONPATH="%PYTHONHOME%\DLLs;%PYTHONHOME%\Lib;%PYTHONHOME%\site-packages" 
PATH="%PATH%;%PYTHONPATH%" 
```

Run Agent-Input and Agent-Output separately...

Caffe.cfg “mode GPU” -> “mode CPU” ?! 


ASR see [ASR/README.md](ASR/README.md)

## How it works

Input (except ASR): SSI https://rawgit.com/hcmlab/ssi/master/docs/index.html 

(ActiveMQ) 

Flipper2 “dialog mgr” 

(ActiveMQ) 

Greta 
- Greta (Java): FML (function) -> BML (behaviour/animation) : MPEG4 Body /Face Animation Parameters -> Ogre3D/ Unity  
- Can be manually/by file assembled in “Modular” 
- For AVP, graph is built and configured programmatically in eu.aria.output.Greta  
- https://github.com/gretaproject/greta/blob/master/README.md 
- Greta/Unity link appears to use Apache Thrift, https://thrift.apache.org/static/files/thrift-20070401.pdf (implied by documentation on Unity C# script, code not publicly accessible) 
- In greta/auxiliary/Thrift 
- (also has greta/auxiliary/ActiveMQ - used with ARIA) 
- (some indication that initial unity support requires audio file of rendered audio for sync in unity) 

TTS (managed from Greta) can be CereProc or OpenMary (http://mary.dfki.de/)  

Note activeMQ UI default http://127.0.0.1:8161/ 
username: admin, password: admin 


## UoN Installers 

https://github.com/msegt/AVP-3_0_1

https://dev.azure.com/MariaGalvezTrigo/AVP_bundle_installer

Seem to be missing executables of most things cf ARIA-System repo

 