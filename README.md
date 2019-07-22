# Get started with Aria-Veluspa Platform - Virtual Human

Chris Greenhalgh, The University of Nottingham, 2019

## install / run

[AVP installation instructions](https://github.com/ARIA-VALUSPA/AVP/wiki/Installation)

clone https://github.com/ARIA-VALUSPA/ARIA-System 
- Includes binaries for windows, at least 

JDK OpenJDK (Oracle now commercial license), 1.8 to be on the safe side 
- https://openjdk.java.net/install/ 
- set JAVA_HOME and PATH

Lots of file permission problems â€“ can't exec DLL and EXE files - just my windows 10 machine??
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
```
cd Agent-Output\bin
java -jar Agent-Output.jar ./environment-ARIADemo2.xml false GRETA
```
and
```
cd Agent-Input\ssi\pipes\all-in-one-final
..\..\bin\xmlpipe.exe -config all-in-one-final -debug ssi.log all-in-one-final
```
(pick camera, confirm settings, pick audio, send/record)

Greta gives this error:
```
[Thread-0] ERROR e.a.d.behaviours.AlignmentGeneration - Timeout or unreachable or failed DNS lookup
```
Tagged as AVP issue [18](https://github.com/ARIA-VALUSPA/AVP/issues/18)

From Core `./src/main/java/eu/aria/dm/behaviours/AlignmentGeneration.java`

It uses some server to modify statements. Why?? What server??

Referenced in DM templates ARIA.xml and FMLManagement.xml

Can probably just remove invocations in ARIA.xml, which add to history,
and switch call in FMLManagement.xml to just set is.behaviour.modifications to `[]`


It seems to fail to play some selected moves?? Perhaps related??? Or are they delayed by timeouts??


? Caffe.cfg mode GPU -> mode CPU ?! 


ASR see [ASR/README.md](ASR/README.md)

## How it works

Input (except ASR): SSI https://rawgit.com/hcmlab/ssi/master/docs/index.html 

(ActiveMQ) 

Flipper2 dialog mgr 

(ActiveMQ) 

Greta 
- Greta (Java): FML (function) -> BML (behaviour/animation) : MPEG4 Body /Face Animation Parameters -> Ogre3D/ Unity  
- Can be manually/by file assembled in "Modular" 
- For AVP, graph is built and configured programmatically in eu.aria.output.Greta  
- https://github.com/gretaproject/greta/blob/master/README.md 
- (also has greta/auxiliary/ActiveMQ - used with ARIA) 
- (some indication that initial unity support requires audio file of rendered audio for sync in unity) 

TTS (managed from Greta) can be CereProc or OpenMary (http://mary.dfki.de/)  

Note activeMQ UI default http://127.0.0.1:8161/ 
username: admin, password: admin 


## UoN Installers 

https://github.com/msegt/AVP-3_0_1

https://dev.azure.com/MariaGalvezTrigo/AVP_bundle_installer

Seem to be missing executables of most things cf ARIA-System repo

## Android notes

SSI has an Android analogue, [SSJ](https://github.com/hcmlab/ssj)

Kaldi can be compiled for Android -
see [this blog post](http://jcsilva.github.io/2017/03/18/compile-kaldi-android/)

## Unity notes

Grata:
- Greta/Unity link appears to use Apache Thrift, https://thrift.apache.org/static/files/thrift-20070401.pdf (implied by documentation on Unity C# script, code not publicly accessible) 
- In greta/auxiliary/Thrift 
- Source for unity component requires SVN repo credentials (not known)
- how about [this git repo](https://github.com/isir/gretaUnity/) ?!

ASAP/Unity bridge
- "An ASAP Realizer-Unity3D Bridge for Virtual and Mixed Reality Applications" IVA 2017? [paper](https://merijnbruijnes.nl/wp-content/uploads/2017/09/Kolkmeier-et-al-2017-An-ASAP-Realizer-Unity3D-Bridge-for-Virtual-and.pdf)
- [source](https://github.com/hmi-utwente/AsapUnityBridge)
- depends on UMA 2 (tested w 2.6.1)
- uses ActiveMQ "apollo" broker to communicate?!

Video
- apparently on Mobile video cannot be used in textures, so how will we show the avatar alongside video?

Unity (in particular UMA 2) seems to have its own skeleton/
animation model, and it looks like these bridges have to do quite a
lot of mapping to use the Unity model (i.e. its not MPEG4 standard).

## Unity and Android notes

Got VideoPlayer working in Android on far clip plane, 
but that doesn't give any option to (e.g.) push it to one edge of the 
screen, or make it smaller. Had to set Player Settings `Auto Graphics API` 
to true to stop it crashing (trying to use Vulcan?).

Not tried VideoPlayer into texture yet - I got the impression it 
might not work, but perhaps that was about the old Video Texture support
in Unity (or might be very slow).

## Flipper (dialog) notes

Flipper reads config from 
[src/main/resources](https://github.com/ARIA-VALUSPA/AVP/tree/master/Agent-Core-Final/src/main/resources)
in particular 
[aria.properties](https://github.com/ARIA-VALUSPA/AVP/blob/master/Agent-Core-Final/src/main/resources/aria.properties)
which in turn lists a set of templates to load, e.g.
```
templates: templates/ARIA.xml, templates/NLU.xml, templates/InteractionManagement.xml, templates/Moves_Eval_Quest1.xml, templates/FMLManagement.xml, templates/SocialTemplates.xml, templates/InterruptionHandling.xml
```

Actual FLM templates in 
[data/fmltemplates](https://github.com/ARIA-VALUSPA/AVP/tree/master/Agent-Core-Final/src/main/resources/data/fmltemplates)

Each seems to be an FML-APML elements with both a BML element (with any utterance) 
and a FML element with affective/intent information.
See also [FML-APML](http://www.mauriziomancini.org/downloads/fml-aamas.pdf)

Note, in ACtiveMQ manager, can list topics, choose (e.g.) `vib.input.FML` and paste in a FML message...
```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE fml-apml SYSTEM ".\Common\Data\ARIA-ValuspaFMLTranslator\fml-apml.dtd">
<fml-apml composition="replace" reaction_duration="none" reaction_type="none" social_attitude="neutral">
	
	<bml>
		<speech id="s1" language="english" start="0.0" text="" voice="cereproc">
			<description level="1" type="gretabml">
				<reference>tmp/from-fml-apml.pho</reference>
			</description>
			
			<tm id="DMBegin"/>
			<voice emotion="none">Hello <tm id="tm1"/>human!</voice>
			<tm id="DMEnd"/>
			
			<pitchaccent end="s1:DMEnd" id="pa1" importance="1" level="moderate" start="s1:tm1" type="Hstar"/>
			<boundary id="b1" start="s1:DMEnd" type="HL"/>
		</speech>
	</bml>
	
	<fml>
		<emotion end="s1:DMEnd+0.1" id="em1" importance="1.0" intensity="1" regulation="felt" start="s1:DMBegin" type="neutral"/>
		<performative end="s1:DMEnd" id="p1" importance="1" start="s1:DMBegin" type="greet"/>
	</fml>
	
</fml-apml>'
```

```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE fml-apml SYSTEM ".\Common\Data\ARIA-ValuspaFMLTranslator\fml-apml.dtd">
<fml-apml composition="replace" reaction_duration="none" reaction_type="none" social_attitude="neutral">
	
	<bml>
		<speech id="s1" language="english" start="0.0" text="" voice="cereproc">
			<description level="1" type="gretabml">
				<reference>tmp/from-fml-apml.pho</reference>
			</description>
			
			<tm id="DMBegin"/>
			<voice emotion="none"> Oxfordshire is where I live, <tm id="DMImpBegin"/> I think.<tm id="DMImpEnd"/></voice>
			<tm id="DMEnd"/>
			
			<pitchaccent end="s1:DMImpEnd" id="pa1" importance="1" level="none" start="s1:DMImpBegin" type="Hstar"/>
			<boundary id="b1" start="s1:DMEnd-0.2" type="HH"/>
		</speech>
	</bml>
	
	<fml>
		
				<emotion end="s1:DMEnd" id="em1" importance="1.0" intensity="1" regulation="felt" start="s1:DMBegin" type="neutral"/>
				<emotion end="s1:DMImpEnd" id="em2" importance="1.0" intensity="0.7" regulation="felt" start="s1:DMImpBegin" type="embarrassment"/>
				<certainty end="s1:DMEnd" id="cr1" importance="0.5" intensity="0.5" start="s1:DMBegin" type="none"/>
				<performative end="s1:DMImpEnd" id="p1" importance="1.0" start="s1:DMImpBegin" type="inform"/>
			
	</fml>
	
</fml-apml>
```
or
```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE fml-apml SYSTEM ".\Common\Data\ARIA-ValuspaFMLTranslator\fml-apml.dtd">
<fml-apml composition="replace" reaction_duration="none" reaction_type="none" social_attitude="neutral">
	
	<bml>
		<speech id="s1" language="english" start="0.0" text="" voice="cereproc">
			<description level="1" type="gretabml">
				<reference>tmp/from-fml-apml.pho</reference>
			</description>
			
			<tm id="DMBegin"/>
			<voice emotion="none">Rock and roll!</voice>
			<tm id="DMEnd"/>
			
			<pitchaccent end="s1:DMImpEnd" id="pa1" importance="1" level="none" start="s1:DMImpBegin" type="Hstar"/>
			<boundary id="b1" start="s1:DMEnd-0.2" type="HH"/>
		</speech>
	</bml>
	
	<fml>
		<emotion end="s1:DMEnd" id="em2" importance="1.0" intensity="0.7" regulation="felt" start="s1:DMBegin" type="embarrassment"/>
		<performative end="s1:DMEnd" id="p1" importance="1.0" start="s1:DMBegin" type="inform"/>
			
	</fml>
</fml-apml>
```